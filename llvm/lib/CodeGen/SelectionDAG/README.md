# DAG Combiner

## Basic Binary Arithmetic Operators

+ https://github.com/abhishek-kaushik22/llvm-project/blob/CGOfficeHours/llvm/include/llvm/CodeGen/ISDOpcodes.h#L245-L252

* visitADD 

- Cases

+ Combine `a+b` to `a|b` if a and b share no bits.

* visitSUB 

* visitMUL 

* visitSDIV 

* visitUDIV 

* visitREM

## Saturated Operators

+ https://github.com/abhishek-kaushik22/llvm-project/blob/CGOfficeHours/llvm/include/llvm/CodeGen/ISDOpcodes.h#L341-L348

* visitADDSAT 

* visitSUBSAT 

## Double Precision Multiplication

+ https://github.com/abhishek-kaushik22/llvm-project/blob/CGOfficeHours/llvm/include/llvm/CodeGen/ISDOpcodes.h#L254-L258

* visitSMUL_LOHI 

* visitUMUL_LOHI 

## Double Precision Multiplication with truncation

+ https://github.com/abhishek-kaushik22/llvm-project/blob/CGOfficeHours/llvm/include/llvm/CodeGen/ISDOpcodes.h#L671-675

* visitMULHU 

* visitMULHS 

## Add/Sub with Carry

+ https://github.com/abhishek-kaushik22/llvm-project/blob/CGOfficeHours/llvm/include/llvm/CodeGen/ISDOpcodes.h#L269-L277

* visitADDC

* visitSUBC

## Averaging Add

+ https://github.com/abhishek-kaushik22/llvm-project/blob/CGOfficeHours/llvm/include/llvm/CodeGen/ISDOpcodes.h#L677-L686

* visitAVG 

## Absolute Difference

+ https://github.com/abhishek-kaushik22/llvm-project/blob/CGOfficeHours/llvm/include/llvm/CodeGen/ISDOpcodes.h#L688-L693

* visitABD 

## Overflow-aware Arithemtic Operators

+ https://github.com/abhishek-kaushik22/llvm-project/blob/CGOfficeHours/llvm/include/llvm/CodeGen/ISDOpcodes.h#L323-L339

* visitADDO 

* visitSUBO 

* visitMULO  

## Arbitrary Large Addition Subtractions Operators

+ https://github.com/abhishek-kaushik22/llvm-project/blob/CGOfficeHours/llvm/include/llvm/CodeGen/ISDOpcodes.h#L279-L287

* visitADDE

* visitSUBE

## Overflow-aware Arithemtic Operators with Carry

+ https://github.com/abhishek-kaushik22/llvm-project/blob/CGOfficeHours/llvm/include/llvm/CodeGen/ISDOpcodes.h$L289-L321

* visitUADDO_CARRY 

* visitSADDO_CARRY 

* visitUSUBO_CARRY 

* visitSSUBO_CARRY

## Fixed point multiplication

+ https://github.com/abhishek-kaushik22/llvm-project/blob/CGOfficeHours/llvm/include/llvm/CodeGen/ISDOpcodes.h#L369-L381

* visitMULFIX 

## Misc.

* visitADDLike 

* visitADDLikeCommutative 

* visitUADDOLike 

* visitUADDO_CARRYLike 

* visitSADDO_CARRYLike 

* visitSDIVLike 

* visitUDIVLike 

* visitIMINMAX 
