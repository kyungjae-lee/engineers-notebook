<a href="../../">Home</a> > <a href="../notebook">Notebook</a> > <a href="./">Linux Kernel Analysis</a> > head.S

# head.S



### Kernel Startup Entry Point

```assembly
/*
 * Kernel startup entry point.
 * ---------------------------
 *
 * The requirements are:
 *   MMU = off, D-cache = off, I-cache = on or off,
 *   x0 = physical address to the FDT blob.
 *
 * Note that the callee-saved registers are used for storing variables
 * that are useful before the MMU is enabled. The allocations are described
 * in the entry routines.
 */
```

Why does the kernel startup entry point not care about the I-cache on/off state?



### Image Header Expected by Linux Bootloaders

```assembly
    /*  
     * DO NOT MODIFY. Image header expected by Linux boot-loaders.
     */
    efi_signature_nop           // special NOP to identity as PE/COFF executable
    b   primary_entry           // branch to kernel start, magic
    .quad   0               	// Image load offset from start of RAM, little-endian
    le64sym _kernel_size_le     // Effective size of kernel image, little-endian
    le64sym _kernel_flags_le    // Informative flags, little-endian
    .quad   0               	// reserved
    .quad   0               	// reserved
    .quad   0               	// reserved
    .ascii  ARM64_IMAGE_MAGIC   // Magic number
    .long   .Lpe_header_offset  // Offset to the PE header.
```

This image header conforms to the header format defined in the documentation `Documentation/arm64/booting.rst` section 4.

`efi_signature_nop` is a macro defined in the file `arch/arm64/kernel/eif-header.S`:

> ```assembly
>     .macro  efi_signature_nop                                           
> #ifdef CONFIG_EFI
> .L_head:
>     /*
>      * This ccmp instruction has no meaningful effect except that
>      * its opcode forms the magic "MZ" signature required by UEFI.
>      */
>     ccmp    x18, #0, #0xd, pl
> #else
>     /*  
>      * Bootloaders may inspect the opcode at the start of the kernel
>      * image to decide if the kernel is capable of booting via UEFI.
>      * So put an ordinary NOP here, not the "MZ.." pseudo-nop above.
>      */
>     nop 
> #endif
> ```
>
> Here `ccmp` instruction is 0xFA405A4D in hex code and this will appear as 4D 5A 40 FA in the kernel image built in little-endian mode, first two of which are 'M' and 'Z' respectively.
>
> The `nop` instruction will simply consume one clock cycle, doing nothing meaningful.



### __INIT

```assembly
    __INIT
```

`__INIT` is a macro defined in `include/linux/init.h`:

> ```c
> #define __INIT      .section    ".init.text","ax" 
> ```



### record_mmu_state

```assembly
SYM_CODE_START_LOCAL(record_mmu_state)
    mrs x19, CurrentEL
    cmp x19, #CurrentEL_EL2
    mrs x19, sctlr_el1
    b.ne    0f  
    mrs x19, sctlr_el2
0:
CPU_LE( tbnz    x19, #SCTLR_ELx_EE_SHIFT, 1f    )
CPU_BE( tbz x19, #SCTLR_ELx_EE_SHIFT, 1f    )
    tst x19, #SCTLR_ELx_C       // Z := (C == 0)
    and x19, x19, #SCTLR_ELx_M      // isolate M bit
    csel    x19, xzr, x19, eq       // clear x19 if Z
    ret 

    /*  
     * Set the correct endianness early so all memory accesses issued
     * before init_kernel_el() occur in the correct byte order. Note that
     * this means the MMU must be disabled, or the active ID map will end
     * up getting interpreted with the wrong byte order.
     */
1:  eor x19, x19, #SCTLR_ELx_EE
    bic x19, x19, #SCTLR_ELx_M
    b.ne    2f  
    pre_disable_mmu_workaround
    msr sctlr_el2, x19 
    b   3f  
2:  pre_disable_mmu_workaround
    msr sctlr_el1, x19 
3:  isb 
    mov x19, xzr 
    ret 
SYM_CODE_END(record_mmu_state)
```

This subroutine records the MMU on/off state in `x19` upon return.

* If `x19` == 0, cache maintenance (e.g., invalidation) will be necessary
* If `x19` == 1, cache maintenance will not be necessary

Think of the following four possible scenarios:

1. **Cache on, MMU on (`x19` = 1)**

   Cache invalidation not necessary since the cache will already contain valid data

2. Cache on, MMU off (`x19` = 0)

   Cache invalidation necessary since the validity of the cache contents will not be guaranteed at the moment when the MMU turns on

3. Cache off, MMU on (`x19` = 0)

   Cache invalidation necessary since the validity of the cache contents is not guaranteed

4. Cache off, MMU off (`x19` = 0)

   Cache invalidation necessary since the validity of the cache contents will not be guaranteed at the moment when the MMU turns on
