# DAG Combiner

## Basic Binary Arithmetic Operators

* https://github.com/abhishek-kaushik22/llvm-project/blob/CGOfficeHours/llvm/include/llvm/CodeGen/ISDOpcodes.h#L245-L252

* visitADD 

    - Cases

        + Combine `a+b` to `a|b` if a and b share no bits.

        + Basic Algebraic optimizations and canonicalizations like

            - Folds `(add x, undef) -> undef`

            - Folds `(add c1, c2) -> c1+c2`

            - Canonicalize constant to RHS

            - Folds `(add x, 0) -> x`

            - (add step_vector(c1), step_vector(c2)  to step_vector(c1+c2))

            - More combines in `visitAddLike`

* visitSUB 

    - Cases

        + Folds `(sub x, x) -> 0`

        + Folds `(sub c1, c2) -> c3`

        + Folds `fold (sub x, c) -> (add x, -c)`

        + Canonicalize (sub -1, x) -> ~x, i.e. (xor x, -1)

        + Tries to replace `sub` with `add` wherever possible for more folding potential

* visitMUL 

    - Cases

        + Folds `(mul x, undef) -> 0`

        + Folds `(mul c1, c2) -> c1*c2`

        + Canonicalize constants to RHS

        + Folds `(mul x, (1 << c)) -> x << c`


* visitSDIV 

    - Cases

        + Constant folding

        + Strength reduces to UDIV is sign bit in both operands are zero

        + If the corresponding remainder node exists, update its users with `Dividend - (Quotient * Divisor)`.

* visitUDIV 

    - Cases 

        + Constant folding

        + Folds `(udiv X, -1) -> select(X == -1, 1, 0)`

        + If the corresponding remainder node exists, update its users with `Dividend - (Quotient * Divisor)`.

* visitREM

    - Cases

        + Constant folding

        + If we know the sign bits of both operands are zero, strength reduce to a `urem` instead

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
