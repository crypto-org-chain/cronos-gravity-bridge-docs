# Gravity Bridge Upgrade guide

## Summary

This document describes the process of migrating to a new gravity contract for the network. While in principle the contract has been audited and secure, it shouldn't be changed. The upgrade process should be only trigger in case of absolute necessity (zero day bug, etcs)

- Step 1: 
Freeze outgoing transaction (no more batch should be created) so that nothing can be send to ethereum. This can be achieved by setting `BatchCreationPeriod` to a very high number or `BatchMaxElement` to zero. 
We need to wait a significant amount of time for all batches to be relayed to Ethereum and empty the batch pool

- Step 2:
Freeze the current gravity contract so that nothing can be sent to cronos. It can be achieved by pausing the contract.

- Step 3: 
Deploy new gravity contract and migrate funds from current contract to new contract. This can be achieved by calling the current contract function `startMigration` and calling `migrateToken` for each tokens.

- Step 4: 
Perform a chain upgrade. An upgrade handler is need in order to set a new gravity id and call the function `MigrateGravityContract` to clean the state





