> ## Table of Contents
>
> - [π νλ‘μΈμ€](#-νλ‘μΈμ€)
>   * [π νλ‘κ·Έλ¨κ³Ό νλ‘μΈμ€ β¨](#-νλ‘κ·Έλ¨κ³Ό-νλ‘μΈμ€-)
>   * [π νλ‘κ·Έλ¨μμ νλ‘μΈμ€λ‘](#-νλ‘κ·Έλ¨μμ-νλ‘μΈμ€λ‘)
>   * [π νλ‘μΈμ€μ μν β¨](#-νλ‘μΈμ€μ-μν-)
>     + [π μ νλ‘μΈμ€μ μνλ§λ€ νλ₯Ό λκ³  κ΄λ¦¬νλ κ±ΈκΉμ?](#-μ-νλ‘μΈμ€μ-μνλ§λ€-νλ₯Ό-λκ³ -κ΄λ¦¬νλ-κ±ΈκΉμ)
>     + [π νλ‘μΈμ€μ μν μ’λ₯](#-νλ‘κ·Έλ¨μμ-νλ‘μΈμ€λ‘)
> - [π PCB: Process Control Block](#-pcb-process-control-block)
>   * [π νλ‘μΈμ€ μ μ΄ λΈλ‘](#-νλ‘μΈμ€-μ μ΄-λΈλ‘)
>     + [βοΈPCB κ΅¬μ±](#pcb-κ΅¬μ±)
>   * [π Context Switching β¨](#-context-switching-)
>     + [βοΈ Context Switching(λ¬Έλ§₯ κ΅ν)μ΄λ](#-context-switchingλ¬Έλ§₯-κ΅νμ΄λ)
>     + [βοΈ μΈμ  μΌμ΄λ κΉ?](#-μΈμ -μΌμ΄λ κΉ)
>     + [βοΈ μ μ°¨](#-μ μ°¨)
>     + [βοΈλ¨μ ](#λ¨μ )
>     + [π μ€λ²ν€λλ₯Ό ν΄κ²°ν  μ μλ λ°©λ²μ΄ μμκΉμ?](#-μ€λ²ν€λλ₯Ό-ν΄κ²°ν -μ-μλ-λ°©λ²μ΄-μμκΉμ)
>
>
> > ### μ¬κΈ°μλ λ³Ό μ μμ΄μ
> > 1. [νλ‘μΈμ€μ κ°μ](https://goldggyul.github.io/os-001-process/)
> > 2. [νλ‘μΈμ€ μ μ΄ λΈλ‘κ³Ό λ¬Έλ§₯ κ΅ν](https://goldggyul.github.io/os-002-pcb/)

----------------------

# π νλ‘μΈμ€

## π νλ‘κ·Έλ¨κ³Ό νλ‘μΈμ€ β¨

μ΄μμ²΄μ μμ `νλ‘μΈμ€`λ νλμ μμλ¨μμλλ€. (κ·Έλμ νλ‘μΈμ€λ₯Ό `task`λΌκ³ λ λΆλ¦λλ€.) μ¬μ©μκ° λ§μ°μ€λ₯Ό λλΈν΄λ¦­νμ¬ `νλ‘κ·Έλ¨`μ μ€ννλ©΄ κ·Έ νλ‘κ·Έλ¨μ΄ νλ‘μΈμ€κ° λλ κ²λλ€. κ·ΈλΌ μ νν λμ μ°¨μ΄λ λ­κΉμ?

- μ μ μΈ μν vs. λμ μΈ μν
  - νλ‘κ·Έλ¨μ μ μ₯μ₯μΉμ λ³΄κ΄λμ΄ μμ΅λλ€. κ·Έλ¦¬κ³  νλ‘κ·Έλ¨μ΄ μ€νλλ€λ κ±΄ νλ‘κ·Έλ¨ μ½λκ° `λ©λͺ¨λ¦¬`μ μ¬λΌμμ μμμ΄ μ§νλλ κ²μλλ€. κ·Έλ¦¬κ³  μ΄λ κ² λ©λͺ¨λ¦¬μ μ¬λΌμ¨ κ²μ΄ νλ‘μΈμ€λΌκ³  ν  μ μμ΅λλ€. 
- νλ‘κ·Έλ¨μ μμ μ μ°¨λ₯Ό μ­ μ μ΄λμ κ²μ΄κ³ , νλ‘μΈμ€λ κ·Έ μμ±λ μμ μ μ°¨λ₯Ό μ€μ λ‘ μ€ννλ κ²μλλ€.
  - μλ₯Ό λ€μλ©΄, μλ¦¬ν  λ λ μνΌλ₯Ό λ³΄κ³  λ μνΌλ₯Ό μμλλ‘ μ€ννλ©΄μ μλ¦¬λ₯Ό νλ κ²κ³Ό κ°μ΅λλ€. μ΄ λ λ μνΌκ° νλ‘κ·Έλ¨μ΄κ³ , μλ¦¬λ₯Ό νλ κ²μ νλ‘μΈμ€κ° λͺλ Ήμ μ€ννλ κ²μ΄λΌκ³  ν  μ μμ΅λλ€.
  - μ΄ λ, λ μ€ν λμμ μλ¦¬νλ μλ¦¬μ¬λ₯Ό μκ°ν΄λ΄μλ€. κ³μν΄μ μ¬λ¬ μλ¦¬ μ£Όλ¬Έμ΄ λ€μ΄μ€κ² μ£ ? κ·Όλ° μλ¦¬μ¬κ° ν λ²μ ν μλ¦¬λ§ μλ¦¬νλ©΄ μ΄λ¨κΉμ? μλλ€λ μ€λ κΈ°λ€λ¦¬κ³ , λΉν¨μ¨μ μΌλ‘ μλ¦¬λ₯Ό νκ² λκ² μ£ ? νλ‘μΈμ€λ λ§μ°¬κ°μ§μλλ€. CPUκ° ν νλ‘μΈμ€κ° μ’λ£λ  λκΉμ§ λ€λ₯Έ νλ‘μΈμ€λ₯Ό μ€ννμ§ λͺ»νλ€λ©΄ λΉν¨μ¨μ μλλ€. κ·Έλμ μ€μ λ‘ νλ‘μΈμ€ μ­μ `Context Switching(λ¬Έλ§₯ κ΅ν)`μ ν΅ν΄ μ¬λ¬ νλ‘μΈμ€κ° λ²κ°μκ°λ©° μ€νλκ² λ©λλ€.

## π νλ‘κ·Έλ¨μμ νλ‘μΈμ€λ‘

λμ μ°¨μ΄λ μκ² μ΅λλ€. κ·ΈλΌ νλ‘κ·Έλ¨μμ μ νν μ΄λ»κ² νλ‘μΈμ€λ‘ μ νλλ κ±ΈκΉμ?

μ°μ  λͺλ Ήμ΄λ₯Ό μ€ννκΈ° μν΄μλ νλ‘κ·Έλ¨μ λ©λͺ¨λ¦¬μ μ¬λ €μΌ νκ² μ£ ? κ·Έλμ νλ‘κ·Έλ¨μ λ©λͺ¨λ¦¬μ μ λΉν μμΉλ‘ κ°μ Έμ΅λλ€. λ©λͺ¨λ¦¬μ κ°μ Έμ€κΈ°λ§ νλ©΄ λμΌκΉμ?

μμμ μ§μ§λ‘ νλ κ±΄ CPUμλλ€. CPUκ° νλ‘μΈμ€λ₯Ό μ€ννκΈ° μν΄μλ νλ‘μΈμ€κ° λ©λͺ¨λ¦¬ μ΄λμ μ¬λΌμ μλ μ§λ₯Ό μμμΌν  κ²μλλ€. λ, μμμ Context Switchingμ ν΅ν΄ μ¬λ¬ νλ‘μΈμ€κ° λ²κ°μκ°λ©° μ€νλλ€κ³  νμ΅λλ€. κ·Έλ λ€λ©΄ μ΄μ μ νλ‘μΈμ€κ° μ΄λκΉμ§ μ€νλλ μ§ μ λ³΄λ νμν©λλ€. λ μ¬λ¬ νλ‘μΈμ€κ° μ€νλκ³  μμΌλ, νλ‘μΈμ€λ§λ€ μ΄λ¦μ΄ μμ΄μΌ μλ‘ κ΅¬λΆνκ³  μνλ κ±Έ μ€νν  μλ μμ κ²μλλ€.

<u>μ¦, λ©λͺ¨λ¦¬μ μ¬λ¦¬λ κ² λΏλ§ μλ μ΄λ° νλ‘μΈμ€λ₯Ό μ€ννλ λ° νμν κ°μ’ μ λ³΄λ€μ μ μ₯ν΄λμΌ ν©λλ€.</u> μ΄ μ λ³΄λ€μ μ΄μμ²΄μ λ `PCB(Process Control Block)`λ₯Ό μμ±ν΄μ κ΄λ¦¬ν©λλ€.

## π νλ‘μΈμ€μ μν β¨

μ¬λ¬ νλ‘μΈμ€λ₯Ό λ²κ°μκ°λ©° μ€ννλ€λ©΄, νμ¬ μ€ν μ€μΈ νλ‘μΈμ€λ μμκ²μ΄κ³ , λκΈ° μ€μ΄κ±°λ, μ€νμ κΈ°λ€λ¦¬λ νλ‘μΈμ€κ° μμ κ²μλλ€. OSλ μ΄λ° νλ‘μΈμ€μ μνλ₯Ό κ΄λ¦¬ν΄μΌ λ€μμ μ΄λ€ νλ‘μΈμ€λ₯Ό μ€νν  μ§ μ μ μκ² μ£ ? 

μ΄λ° <u>νλ‘μΈμ€μ μνλ§λ€ OSλ μ¬λ¬ νμμ queueλ₯Ό λκ³  κ΄λ¦¬</u>ν©λλ€.

- μ¬κΈ°μ  queueλΌκ³  νμ§λ§, μ€μ λ‘λ  OSλ§λ€ λ€λ₯΄κ² κ΅¬νλ  μ μμ΅λλ€. μλ₯Ό λ€μ΄ linuxλ Red Black Treeλ₯Ό μ΄μ©ν©λλ€.

κ·Έλ¦¬κ³  μ΄ queueμ PCBλ₯Ό λ£μ΄μ κ΄λ¦¬νλ κ²μλλ€.

> ### π μ νλ‘μΈμ€μ μνλ§λ€ νλ₯Ό λκ³  κ΄λ¦¬νλ κ±ΈκΉμ?

### π νλ‘μΈμ€μ μν μ’λ₯

ν¬κ²λ Run, Ready, Waiting 3κ°μ§ μνλ‘ λ³Ό μλ μκ³ , μ΄μΈμλ μ¬λ¬ νλ‘μΈμ€ μν λͺ¨λΈμ΄ μλλ°, 5κ°μ§ μνμ νλ‘μΈμ€ λͺ¨λΈμ μ΄ν΄λ³΄κ² μ΅λλ€.

![image-20220912042405286](./image/image-20220912042405286.png)

1. μμ± μν `create`
   - νλ‘μΈμ€κ° λ©λͺ¨λ¦¬μ μ¬λΌμ μ€ν μ€λΉλ₯Ό μλ£ν μνμλλ€.
   - μ΄ λ PCBλ μμ±λ©λλ€.
2. μ€λΉ μν `ready`
   - μμ±λ νλ‘μΈμ€κ°  CPUλ₯Ό μ»μ λκΉμ§ κΈ°λ€λ¦¬λ μν. μκΈ° μ€ν μμκ° λ  λκΉμ§ νλ‘μΈμ€λ μ€λΉ μνμμ κΈ°λ€λ €μΌ ν©λλ€.
   - CPU schedulerκ° μ€λΉ νλ₯Ό κ΄λ¦¬ν©λλ€. CPU μ€μΌμ€λ¬λ Context Switchingμ΄ μΌμ΄λ  λ μ΄λ€ νλ‘μΈμ€λ₯Ό running μν¬ μ§ κ²°μ ν©λλ€. λ μμΈν κ±΄ μ€μΌμ€λ¬ κ΄λ ¨ν΄μ λ°λ‘ μ λ¦¬νκ² μ΅λλ€.
3. μ€ν μν `running`
   - μ€λΉ μνμ μλ νλ‘μΈμ€ μ€ νλκ° CPUλ₯Ό μ»μ΄ μ€μ  μμμ μννλ μνμλλ€.
   - μΌμ  μκ°λμ CPUλ₯Ό μ¬μ©ν κΆλ¦¬λ₯Ό κ°λλ€. μ£Όμ΄μ§ μκ°λμ μμμ΄ λλμ§ μμλ€λ©΄ λ€μ μ€λΉ μνλ‘ λμκ°λλ€.
4. μλ£ μν `terminate` `exit`
   - μ£Όμ΄μ§ μκ°λμ μμμ μλ£ν μνμλλ€.
   - μ΄ λ PCBλ νκΈ°νκ³ , μ½λμ μ¬μ©νλ λ°μ΄ν°λ₯Ό λ©λͺ¨λ¦¬μμ μ­μ ν©λλ€.
   - μ μμ  μ’λ£λΌλ©΄ exit()λ‘ κ°λ¨νκ² μ²λ¦¬λμ§λ§, μ€λ₯λ λ€λ₯Έ νλ‘μΈμ€μ μν΄ λΉμ μμ μΌλ‘ μ’λ£λλ κ°μ  μ’λ£(abort)λ₯Ό λ§λλ©΄ λλ²κΉνκΈ° μν΄ κ°μ  μ’λ£ μ§μ μ λ©λͺ¨λ¦¬ μνλ₯Ό μ μ₯μ₯μΉλ‘ μ?κΉλλ€. μ΄λ₯Ό μ½μ΄ λ€ν(core dump)λΌκ³  ν©λλ€.
5. λκΈ° μν `blocking`  `waiting`
   - νλ‘μΈμ€κ° μμΆλ ₯μ μκ΅¬νλ©΄, CPUκ° μ§μ  λ°μ΄ν°λ₯Ό κ°μ Έμ€λ κ²μ΄ μλκ³  μμΆλ ₯ κ΄λ¦¬μμκ² λͺλ Ήμ λ΄λ¦½λλ€. μ΄ μνμμ νλ‘μΈμ€κ° μμ²­ν μμμ΄ λλ  λκΉμ§ CPUκ° μλ¬΄ μμλ νμ§ μκ³  κΈ°λ€λ¦¬λ©΄ ν¨μ¨μ±μ΄ λ¨μ΄μ§κ² μ£ ?
   - λ°λΌμ μμΆλ ₯μ μκ΅¬ν νλ‘μΈμ€κ° μμΆλ ₯μ΄ μλ£λ  λκΉμ§ κΈ°λ€λ¦¬λ μνμλλ€.
   - λκΈ° μνμ νλ‘μΈμ€λ μμ²­ν μμΆλ ₯μ΄ μλ£λλ©΄ μμΆλ ₯ κ΄λ¦¬μλ‘λΆν° μΈν°λ½νΈλ₯Ό λ°μ΅λλ€. νμ¬ μ€ν μ€μΈ νλ‘μΈμ€κ° μκΈ° λλ¬Έμ, λκΈ° μνμ νλ‘μΈμ€λ λ°λ‘ μ€ν μνλ‘ λμκ°μ§ μκ³ , μ€λΉ μνλ‘ λμκ°μ λ€μ μκΈ° μ°¨λ‘λ₯Ό κΈ°λ€λ¦½λλ€.



# π PCB: Process Control Block

## π νλ‘μΈμ€ μ μ΄ λΈλ‘

[νλ‘μΈμ€μ κ°μ](https://goldggyul.github.io/os-001-process/)μμ νλ‘μΈμ€λ₯Ό μ€ννλ λ° λ©λͺ¨λ¦¬λ§ νμν κ²μ΄ μλκ³ , κ΄λ¦¬νκΈ° μν μλ£ κ΅¬μ‘°λ‘ PCBκ° νμνλ€κ³  νμ£ ? PCBμ λν΄ λ μμλ΄μλ€.

- νλ‘μΈμ€λ₯Ό  TaskλΌκ³ λ νλ κ²μ²λΌ, PCB μ­μ TCBλΌκ³ λ λΆλ¦λλ€.

### βοΈPCB κ΅¬μ±

<table style="text-align: center;">
  <tr>
    <td><strong>ν¬μΈν°</strong></td>
    <td>νλ‘μΈμ€ μν</td>
  </tr>
  <tr>
    <td colspan="2">Process ID</td>
  </tr>
  <tr>
    <td colspan="2">Process Priority</td>
  </tr>
    <tr>
    <td colspan="2">Program Counter</td>
  </tr>
  <tr>
    <td colspan="2">κ°μ’ CPU Registers μ λ³΄</td>
  </tr>
  <tr>
    <td colspan="2">λ©λͺ¨λ¦¬ κ΄λ¦¬ μ λ³΄</td>
  </tr>
  <tr>
    <td colspan="2">ν λΉλ μμ μ λ³΄</td>
  </tr>
  <tr>
    <td colspan="2">PPIDμ CPID</td>
  </tr>


</table>

1. ν¬μΈν°
   - [νλ‘μΈμ€μ κ°μ](https://goldggyul.github.io/os-001-process/)μμ νλ‘μΈμ€μ μνμ λ°λΌ νλ‘ μ΄μλλ€κ³  νμ΅λλ€. μ΄ λ queueμμ PCBλ₯Ό μ°κ²°ν  λ ν¬μΈν°λ₯Ό μ¬μ©ν©λλ€.
   
2. νλ‘μΈμ€ μν
   - Run, Ready, Waiting λ±μ μν
   
3. νλ‘μΈμ€ ID
   - νλ‘μΈμ€λ₯Ό κ΅¬λ³νκΈ° μν κ΅¬λΆμ
   
4. νλ‘κ·Έλ¨ μ°μ μμ
   - CPU Schedulingμ΄ μΌμ΄λμ μ€μΌμ€λ¬κ° μ€λΉ μνμ μλ νλ‘μΈμ€ μ€ μ€ν μνλ‘ μ?κ²¨μΌ ν  νλ‘μΈμ€λ₯Ό μ΄λ»κ² μ νν κΉμ?
   - νλ‘μΈμ€ μ°μ μμλ₯Ό κΈ°μ€μΌλ‘ μΌμμ, λμ μ°μ μμμ νλ‘μΈμ€λ₯Ό λ¨Όμ  μ€ννκ² λ©λλ€. μ΄μ κ΄νκ±΄ μ€μΌμ€λ§λ§ λ€λ£° λ λ μμΈν μ΄ν΄λ³΄κ² μ΅λλ€.
   
5. νλ‘κ·Έλ¨ μΉ΄μ΄ν°
   - λ€μμ μ€νλ  λͺλ Ήμ΄μ μμΉλ₯Ό κ°λ¦¬ν€λ νλ‘κ·Έλ¨ μΉ΄μ΄ν° κ°μ μ μ₯ν΄λμ΅λλ€. μ΄κ² μμ΄μΌ Context Switchingμ΄ μΌμ΄λμ λ€μ νλ‘μΈμ€κ° μ€νλ  λ μ΄λλ₯Ό μ€νν΄μΌν  μ§ μ μ μκ² μ£ ?
   
6. κ°μ’ λ μ§μ€ν° μ λ³΄
   - νλ‘μΈμ€κ° μ€νλλ©΄μ ν¨μλ₯Ό νΈμΆν  λλ§λ€ μ§μ­ λ³μλ μ€νμ μ μ₯λμ£ ? κ·ΈλΌ μ΄ μ€νμ΄ μ΄λ¨λ μ§λ₯Ό μμμΌ μ μ₯μ ν  μ μμ΅λλ€. μ΄ λ `μ€ν ν¬μΈν°`λ μ΄ λ μ§μ€ν° μ λ³΄μ ν¬ν¨λ©λλ€.
   - μ΄ κ°μ μ μ₯ν΄λμΌ λ€μμ μ€νν  λ μ΄μ΄μ μ€νν  μ μμ κ±°μμ.

7. λ©λͺ¨λ¦¬ κ΄λ¦¬ μ λ³΄
   - ν λΉλ°μλ λ©λͺ¨λ¦¬κ° μ΄λ μλμ§ λνλ΄λ μμΉ μ λ³΄, `segmentation table`, `page table`μ μ λ³΄λ λ³΄κ΄ν©λλ€. λ μμΈν κ±΄ κ°μ λ©λͺ¨λ¦¬μ λν΄ λ€λ£° λ μ λ¦¬νκ² μ΅λλ€.
   
8. ν λΉλ μμ μ λ³΄
   - νλ‘μΈμ€κ° μ€ν μ€μ μ¬μ©νλ νμΌ, μμΆλ ₯ μ₯μΉ κ°μ λ¦¬μμ€λ μ²μ μ¬μ©λ  λ OSκ° λ¦¬μμ€ λ²νΈλ₯Ό ν λΉν©λλ€. μ΄ λ μ΄ λ²νΈλ μμ μ μλ‘μ, PCBμ file descriptor tableμ λ±λ‘λ©λλ€. 
   - μ°Έκ³ : [bash-shell/file_descriptors](https://mug896.github.io/bash-shell/file_descriptors.html)
   
9. PPIDμ CPID
   - λΆλͺ¨ νλ‘μΈμ€λ₯Ό κ°λ¦¬ν€λ PPID, μμ νλ‘μΈμ€(λ€)λ₯Ό κ°λ¦¬ν€λ CPID μ λ³΄λ μ μ₯λ©λλ€.
   
     >μλ₯Ό λ€μ΄ `fork()` μμ€ν νΈμΆλ‘ νλ‘μΈμ€κ° λ³΅μ¬λ©λλ€. μ΄ λ μλ μ€ννλ νλ‘μΈμ€λ λΆλͺ¨ νλ‘μΈμ€, μλ‘ μκΈ΄ νλ‘μΈμ€λ μμ νλ‘μΈμ€λ‘ λΆλͺ¨-μμ κ΄κ³κ° λ©λλ€.

## π Context Switching β¨

CPUκ° ν νλ‘μΈμ€κ° λλ  λκΉμ§ λ€λ₯Έ νλ‘μΈμ€λ₯Ό μ€ννμ§ μμΌλ©΄ λΉν¨μ¨μ μ΄μ΄μ  μ¬λ¬ νλ‘μΈμ€λ₯Ό λ²κ°μκ°λ©΄μ μ€ννκ³ , κ·Έ λ νλ‘μΈμ€μ μ λ³΄λ€μ PCBμ μ μ₯ν΄μ κ΄λ¦¬νλ€κ³  νμ΅λλ€. κ·ΈλΌ <u>μ΄λ»κ² λ²κ°μκ°λ©΄μ μ€νν κΉμ?</u>

### βοΈ Context Switching(λ¬Έλ§₯ κ΅ν)μ΄λ

Context Switchingμ΄λ CPUλ₯Ό μ°¨μ§νλ νλ‘μΈμ€κ° λκ°κ³  μλ‘μ΄ νλ‘μΈμ€λ₯Ό λ°μλ€μ΄λ μμμλλ€. μ΄ λ μ€ν μνμμ λκ°λ PCBμλ μ§κΈκΉμ§μ μμ λ΄μ©μ μ μ₯νκ³ , λ°λλ‘ μ€ν μνλ‘ λ€μ΄μ€λ PCBμ λ΄μ©μ μ½μ΄μ λ μ§μ€ν°μ μ μ¬νμ¬ CPUκ° μΈνλ©λλ€. κ·ΈλμΌ λ€μ΄μ€λ νλ‘μΈμ€κ° κ³μν΄μ μ΄μ΄μ μμμ ν  μ μκ² μ£ ?

μ¦, μ΄μ κ°μ΄ λ νλ‘μΈμ€μ  PCBλ₯Ό κ΅ννλ μμμ΄ Context Switchingμλλ€. κ·Έλ¦¬κ³  μ΄ λ κ΅μ²΄λλ μλ‘μ΄ νλ‘μΈμ€λ CPU μ€μΌμ€λ¬μ μν΄ κ²°μ λ©λλ€.

### βοΈ μΈμ  μΌμ΄λ κΉ?

μΈν°λ½νΈκ° λ°μνμ λ μΌμ΄λ©λλ€.

- μΌλ°μ μΌλ‘λ νλ‘μΈμ€κ° μ£Όμ΄μ§ CPU μ¬μ© νκ° μκ°μ λͺ¨λ μμ§ν  λ

- μμΆλ ₯μ μν΄ λκΈ°ν  λ

- μμ νλ‘μΈμ€λ₯Ό λ§λ€ λ
- μΈν°λ½νΈ μ²λ¦¬λ₯Ό κΈ°λ€λ¦΄ λ

### βοΈ μ μ°¨

νλ‘μΈμ€ P1κ³Ό  P2μ Context switching κ³Όμ μ μ’ λ μ΄ν΄λ΄μλ€.

![image-20220912042427164](./image/image-20220912042427164.png)

| λ¨κ³ | μ μ°¨                          | μ€λͺ                                                         |
| ---- | ----------------------------- | ------------------------------------------------------------ |
| 1    | μΈν°λ½νΈ/ μμ€ν νΈμΆ         | μ΄λ€ μ΄μ λ‘ μΈν°λ½νΈκ° λ°μν©λλ€. μλ₯Ό λ€μ΄, μ€ν μνμ μλ νλ‘μΈμ€ P1μ΄ μ£Όμ΄μ§ μκ°μ λ€ μ¬μ©νμ¬ μ΄μμ²΄μ μμ μ€μΌμ€λ¬μ μν΄ μΈν°λ½νΈκ° λ°μν©λλ€. |
| 2    | μ»€λ λͺ¨λ μ ν                | P1μ΄ μ μ  λͺ¨λμμ μ»€λ λͺ¨λλ‘ μ ν, P1μ μ€λΉ μν          |
| 3    | νμ¬ νλ‘μΈμ€ μν PCBμ μ μ₯ | P1μ νμ¬ νλ‘μΈμ€ μν PCBμ μ μ₯                           |
| 4    | λ€μ μ€ν νλ‘μΈμ€ λ‘λ       | P2μ PCB μ λ³΄λ₯Ό ν΅ν΄ CPU λ μ§μ€ν° μ λ³΄λ₯Ό μ±μ<br /><br />μ΄ λ Program Counterλ₯Ό ν΅ν΄ μ΄λ λͺλ Ήμ΄λ₯Ό μ€νν  μ§λ₯Ό μ μ μκ³ , Stack Pointerλ₯Ό ν΅ν΄ νλ‘μΈμ€μ μ€ν μμ­ λ§μ§λ§ μ£Όμλ₯Ό μ μ μμ΅λλ€. |
| 5    | μ μ  λͺ¨λ μ ν                | P2 νλ‘μΈμ€ μ»€λ λͺ¨λμμ μ μ  λͺ¨λλ‘ μ νλμ΄ μ€ν          |

### βοΈλ¨μ 

Context Switchingμ΄ λλ¬΄ μ¦μΌλ©΄ μ€λ²ν€λκ° λ°μν΄μ μ±λ₯μ΄ λ¨μ΄μ§λλ€.

- μ»€λ λͺ¨λ, μ μ  λͺ¨λ μ ννλ©΄μ μν μ μ₯νκ³  λ μ§μ€ν°κ° λ€μ λΆλ¬μ€κ³ ..
- κ·Έ μΈμλ μΊμ λ©λͺ¨λ¦¬λ λΉμμΌ νκ³ ..
- μ΄λ¬λ λμ  CPUλ λ€λ₯Έ μμμ νμ§ λͺ»ν©λλ€.

κ·ΈλΌμλ λΆκ΅¬νκ³  CPUλ₯Ό κ·Έλ₯ λκ² λλλ κ²λ³΄λ€ λ€λ₯Έ νλ‘μΈμ€λ₯Ό μνμν€λ κ²μ΄ λ ν¨μ¨μ μΌ λ μ€μΌμ€λ¬λ Context Switchingμ ν©λλ€.

> ### π μ€λ²ν€λλ₯Ό ν΄κ²°ν  μ μλ λ°©λ²μ΄ μμκΉμ?

-----------------------

> π λ€μμ μ°Έκ³ νμ¬ μμ±νμ΅λλ€.
>
> - [μ½κ² λ°°μ°λ μ΄μμ²΄μ ](http://www.yes24.com/Product/Goods/62054527)
> - νλΆ μμ
> - [@gyoogle/tech-interview-for-developer](https://github.com/gyoogle/tech-interview-for-developer/)
