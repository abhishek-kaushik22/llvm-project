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

* Note: Most of the vector patterns for these ops are simplified in `SimplifyVBinOp`

## Saturated Operators

+ https://github.com/abhishek-kaushik22/llvm-project/blob/CGOfficeHours/llvm/include/llvm/CodeGen/ISDOpcodes.h#L341-L348

* visitADDSAT 
* visitSUBSAT 

    - Cases 

        + Strength reduction to `add`/`sub` if operands will not overflow

        + Constant folding

        + Canonicalization of constant to RHS

        + Basic algebraic optimizations

## Double Precision Multiplication

+ https://github.com/abhishek-kaushik22/llvm-project/blob/CGOfficeHours/llvm/include/llvm/CodeGen/ISDOpcodes.h#L254-L258

* visitSMUL_LOHI 

* visitUMUL_LOHI 

    - Cases

        + If the type is twice as wide is legal, transform the `mulhu` to a wider multiply plus a shift.

## Double Precision Multiplication with truncation

+ https://github.com/abhishek-kaushik22/llvm-project/blob/CGOfficeHours/llvm/include/llvm/CodeGen/ISDOpcodes.h#L671-675

* visitMULHU 

* visitMULHS 

    - Cases

        + If the type twice as wide is legal, transform the `mulhu` to a wider multiply plus a shift.

## Add/Sub with Carry

+ https://github.com/abhishek-kaushik22/llvm-project/blob/CGOfficeHours/llvm/include/llvm/CodeGen/ISDOpcodes.h#L269-L277

* visitADDC

* visitSUBC

    - Cases

        + If the flag result is dead, turn this into an `ADD`/`SUB`.

        + If it cannot overflow, transform into an `add`.

## Averaging Add

+ https://github.com/abhishek-kaushik22/llvm-project/blob/CGOfficeHours/llvm/include/llvm/CodeGen/ISDOpcodes.h#L677-L686

* visitAVG 

    - Cases

        + Folds `avgfloor((add nw x,y), 1) -> avgceil(x,y)`
        
        + Folds `avgfloor((add nw x,1), y) -> avgceil(x,y)`

## Absolute Difference

+ https://github.com/abhishek-kaushik22/llvm-project/blob/CGOfficeHours/llvm/include/llvm/CodeGen/ISDOpcodes.h#L688-L693

* visitABD 

    - Cases

        + Folds `(abds x, y) -> (abdu x, y)` if both args are known positive

## Overflow-aware Arithemtic Operators

+ https://github.com/abhishek-kaushik22/llvm-project/blob/CGOfficeHours/llvm/include/llvm/CodeGen/ISDOpcodes.h#L323-L339

* visitADDO 

* visitSUBO 

* visitMULO  

    - Cases
        
        + If the flag result is dead, turn this into the corresponding operation.

        + fold (saddo (xor a, -1), 1) -> (ssub 0, a)

        + `(mulo x, 2) -> (addo x, x) // FIXME: This needs a freeze.`

## Operators For Arbitrary Large Addition Subtractions

+ https://github.com/abhishek-kaushik22/llvm-project/blob/CGOfficeHours/llvm/include/llvm/CodeGen/ISDOpcodes.h#L279-L287

* visitADDE

* visitSUBE

    - Cases 
    
        + Folds `(Ope x, y, false) -> (Opc x, y)`

## Overflow-aware Arithemtic Operators with Carry

+ https://github.com/abhishek-kaushik22/llvm-project/blob/CGOfficeHours/llvm/include/llvm/CodeGen/ISDOpcodes.h$L289-L321

* visitUADDO_CARRY 

* visitSADDO_CARRY 

* visitUSUBO_CARRY 

* visitSSUBO_CARRY

    - Cases

        + Folds `(op_carry x, y, false) -> (op x, y)`

    - `combineUADDO_CARRYDiamond`

        + https://github.com/abhishek-kaushik22/llvm-project/blob/CGOfficeHours/llvm/lib/CodeGen/SelectionDAG/DAGCombiner.cpp:#L3441-L3462

## Fixed point multiplication

+ https://github.com/abhishek-kaushik22/llvm-project/blob/CGOfficeHours/llvm/include/llvm/CodeGen/ISDOpcodes.h#L369-L381

* visitMULFIX 

## 

+ https://github.com/abhishek-kaushik22/llvm-project/blob/CGOfficeHours/llvm/include/llvm/CodeGen/ISDOpcodes.h:#L695-700

* visitIMINMAX 

## Misc.

* visitADDLike 

* visitADDLikeCommutative 

* visitUADDOLike 

* visitUADDO_CARRYLike 

* visitSADDO_CARRYLike 

* visitSDIVLike 

* visitUDIVLike 
