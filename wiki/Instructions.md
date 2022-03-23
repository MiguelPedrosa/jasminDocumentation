## Instrution Index:
 1. [Store Instructions](#Store-Instructions)
 2. [Load Instructions](#Load-Instructions)
 3. [Stack manipulation](#Stack-manipulation)
 4. [Arithmetic and Logical](#Arithmetic-and-Logical)
 5. [Conversions](#Conversions)
 6. [Conditional and unconditional branches](#Conditional-and-unconditional-branches)

#### Sections that haven't been processed 
 6. Comparisions (lcmp, etc.)
 7. Creation and manipulation of objects and arrays (new, newarray, etc.)
 8. Access fields of an object (putfield, getfield, etc.)
 9. Method calls (invokeinterface, invokespecial, etc.)
 10. Constants (iconst_1, ldc “Hello”, etc.)
 11. Switch (lookupswitch e tableswitch)
 12. Definition of synchronous regions (monitorenter e monitorexit)


## Store Instructions

| Instructions name               | Argument       | Description |
| ------------------------------- | -------------- | ----------- |
| `astore`*                       | Non-negative integer | Pops a reference to an object from the top of the stack and stores in the specified local index  |
| `dstore` `fstore` `istore` `lstore`* | Non-negative integer | Pops a double/float/int/long from the top of the stack and stores it in the specified local index |
| `aastore` `bastore` `castore` `dastore` `fastore` `iastore` `lastore` `sastore` | No arguments | In a stack of the form of [a, b, c, ...], where a is the top value, the instruction performs the equivalent of `c[b] = a`. This means the array reference must be the first pushed to the stack, then the element index and lastly the value to be stored. |

\* These instructions have a variant to specify indexs 0 to 3: `astore_0`, `dstore_1`, `fstore_2`, `istore_3`, `lstore_3`, etc.. In case of using these instructions no argument is needed. They might be more efficient to use, but this has yet to be confirmed.


## Load Instructions

| Instructions name           | First Argument   | Description |
| --------------------------- | ---------------- | ----------- |
| `aload`*                    | Non-negative integer | Pushes a reference to an object from the specified local index to the top of the stack |
| `dload` `fload` `iload` `lload`* | Non-negative integer | Pushes a double/float/int/long from the specified local index to the top of the stack |
| `aaload` `baload` `caload` `daload` `faload` `iaload` `laload` `saload` | - | In a stack of the form of `[a, b, ...]`, where a is the top value, the instruction performs the equivalent of `b[a]`. This means the array reference must be the first pushed to the stack and lastly the element's index. |

None of these instructions have a second argument.

\* These instructions have a variant to specify indexs 0 to 3: `aload_0`, `dload_1`, `fload_2`, `iload_3`, `lload_3`, etc.. In case of using these instructions no argument is needed. They might be more efficient to use, but this has yet to be confirmed.


## Stack Manipulation

| Instructions name           | First Argument | Description |
| --------------------------- | -------------- | ----------- |
| `dup`                       | -              | Creates a copy of the topmost element in the stack and push it onto the stack. A stack like `[a, ...]`, becomes `[a, a, ...]`. |
| `dup2`                      |                |             |
| `dup2_x1`                   |                |             |
| `dup2_x2`                   |                |             |
| `dup_x1`                    |                |             |
| `dup_x2`                    |                |             |
| `pop`                       | -              | Pops the topmost element of the stack without using it. A stack like `[a, ...]`, becomes `[...]`. |
| `pop2`                      |                |             |
| `swap`                      | -              | swaps the top two values in the stack. A stack in the form of `[a, b, ... ]` becomes `[b, a, ... ]` |
| `bipush` `sipush`           | Float/Short    | Pushes the argument onto the stack |

None of these instructions have a second argument.


## Arithmetic and Logical

| Operation      | double | float  | integer | long   | Starting Stack | Ending Stack |
| -------------- | ------ | ------ | ------- | ------ | -------------- | ------------ |
| Adition        | `dadd` | `fadd` | `iadd`  | `ladd` | `[a, b, ...]`  | `[b*a, ...]` |
| Subtraction    | `dsub` | `fsub` | `isub`  | `lsub` | `[a, b, ...]`  | `[b-a, ...]` |
| Multiplication | `dmul` | `fmul` | `imul`  | `lmul` | `[a, b, ...]`  | `[b*a, ...]` |
| Quotient       | `ddiv` | `fdiv` | `idiv`  | `ldiv` | `[a, b, ...]`  | `[b/a, ...]` |
| Remainder      | `drem` | `frem` | `irem`  | `lrem` | `[a, b, ...]`  | `[b%a, ...]` |
| Negation       | `dneg` | `fneg` | `ineg`  | `lneg` | `[a, ...]`     | `[-a, ...]`  |

### Bitwise operations

| Operation    | integer | long    | Starting Stack | Ending Stack  |
| -------------| ------- | ------  | -------------- | ------------  |
| And          | `iand`  | `land`  | `[a, b, ...]`  | `[b&a, ...]`  |
| Or           | `ior`   | `lor`   | `[a, b, ...]`  | `[b\|a, ...]` |
| Xor          | `ixor`  | `lxor`  | `[a, b, ...]`  | `[b^a, ...]`  |
| Left-Shift   | `ishl`  | `lshl`  | `[a, b, ...]`  | `[a<<b, ...]` |
| Right-Shift* | `ishr`  | `lshr`  | `[a, b, ...]`  | `[a>>b, ...]` |
| Right-Shift* | `iushr` | `lushr` | `[a, b, ...]`  | `[a>>b, ...]` |

\* The two `shr` instructions do sign extension. The `ushr` don't, as they work with unsigned numbers.

None of these operations take any arguments. They will pop the 2 topmost values of the stack, perform the operation and push the result onto the stack. The exception is the negation that only uses 1 value.


## Conversions

| From\To | double | float | integer | long  | byte             | char             | short             |
| ------  | ------ | ----- | ------- | ----- | -                | ---------------- | ----------------- |
| double  | -      | `d2f` | `d2i`   | `d2l` | -                | -                |                   |
| float   | `f2d`  | -     | `f2i`   | `f2l` | -                | -                |                   |
| integer | `i2d`  | `i2f` | -       | `i2l` | `int2byte` `i2b` | `int2char` `i2c` | `int2short` `i2s` |
| long    | `l2d`  | `l2f` | `l2i`   | -     | -                | -                | -                 |

All of these instructions take the topmost value of the stack assuming the type of the instruction prefix, convert it into the type of the suffix and pushes it into the stack. None of these instructions take arguments.


## Conditional and unconditional branches

| Jump name   | Argument | Type      | Condition | Starting Stack |
| ----------- | -------- | --------- | --------- | -------------- |
| `if_acmpeq` | label    | reference | `b == a`  | `[a, b, ...]`  |
| `if_acmpne` | label    | reference | `b != a`  | `[a, b, ...]`  |
| `if_icmpeq` | label    | integer   | `b == a`  | `[a, b, ...]`  |
| `if_icmpne` | label    | integer   | `b != a`  | `[a, b, ...]`  |
| `if_icmpgt` | label    | integer   | `b >  a`  | `[a, b, ...]`  |
| `if_icmplt` | label    | integer   | `b <  a`  | `[a, b, ...]`  |
| `if_icmpge` | label    | integer   | `b >= a`  | `[a, b, ...]`  |
| `if_icmple` | label    | integer   | `b <= a`  | `[a, b, ...]`  |

These instructions use the 2 topmost values in the stack againt the condition and, if it is true, perform the jump to the label given as an argument.

| Jump name     | Argument | Type      | Condition    | Starting Stack |
| ------------- | -------- | --------- | ------------ | -------------- |
| `ifnonnull`   | label    | reference | `a != null`  | `[a, ...]`     |
| `ifnull`      | label    | reference | `a == null`  | `[a, ...]`     |
| `ifeq`        | label    | integer   | `a == 0`     | `[a, ...]`     |
| `ifge`        | label    | integer   | `a >= 0`     | `[a, ...]`     |
| `ifgt`        | label    | integer   | `a >  0`     | `[a, ...]`     |
| `ifle`        | label    | integer   | `a <= 0`     | `[a, ...]`     |
| `iflt`        | label    | integer   | `a <  0`     | `[a, ...]`     |
| `ifne`        | label    | integer   | `a != 0`     | `[a, ...]`     |

These instructions use the topmost value in the stack againt the condition and, if it is true, perform the jump to the label given as an argument.

| Jump name     | Argument | Condition    |
| ------------- | -------- | ------------ |
| `jsr` `jsr_w` | Label    |              |
| `goto`        | Label    | always jumps |
| `goto_w`      | Label    | always jumps |

I do not know the difference between `goto` and `goto_w`. The instructions `jsr` and `jsr_w` are marked in the source code as synonym.


### Instructions that haven't been documented

aconst_null
anewarray
areturn
arraylength
athrow
breakpoint
checkcast
dcmpg
dcmpl
dconst_0
dconst_1
dreturn
fcmpg
fcmpl
fconst_0
fconst_1
fconst_2
freturn
getfield
getstatic
iconst_0
iconst_1
iconst_2
iconst_3
iconst_4
iconst_5
iconst_m1
iinc
instanceof
invokeinterface
invokenonvirtual
invokespecial // added this synonym
invokestatic
invokevirtual
ireturn
lcmp
lconst_0
lconst_1
ldc
ldc_w
ldc2_w
lookupswitch
lreturn
monitorenter
monitorexit
multianewarray
new
newarray
nop
putfield
putstatic
ret
ret_w // synonym for ret
return
tableswitch
wide
