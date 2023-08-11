<a href="../../">Home</a> > <a href="../notebook">Notebook</a> > <a href="./">Direct Memory Access (DMA)</a> > DMA Programming - Generic Steps to Follow

# DMA Programming - Generic Steps to Follow



## Generic Steps to Follow while using DMA in an Application

1. Identify the DMAx controller suitable for your application (Capability of each DMAx supported by your MCU may vary)
2. Initialize the DMA
3. Trigger the DMA (Manual or automatic trigger)
   * Manual trigger - Kick starting the DMA engine with the help of a processor core
   * Automatic trigger (Desirable) - Kick starting the DMA engine with help of a peripheral but not the processor core
4. Wait for Transmission Complete (TC) flag (i.e., polling) or get callback from DMA driver (i.e., interrupt)
