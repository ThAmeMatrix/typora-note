

# CSE 131 - Compiler Construction - LE [A00]

- **Professor Politz, Joseph Gibbs**
- **Spring 2018**
- **Lecture 24, 5/25/2018  9:00 AM-Center Hall 109**



# Slide 1

![1](/home/shiro/Desktop/Translate/img/1.png)

```ocaml
| EIf(ifexpr, thenexpr, elseexpr) ->
	let _ = update_depth si in
	let test_bool = 
		[ IMov(stackloc si, Reg(EAX));
		  IAnd(Reg(EAX), Const(1));
		  ICmp(Reg(EAX), Const(0));
		  IJne(error_non_bool);
		  IMov(Reg(EAX), stackloc si);] in
	let ifexpr = compile_expr ifexpr si env in
	let thenexpr = compile_expr thenexpr si env in
	let elseexpr = compile_expr elseexpr si env in
	let else_lbl = gen_temp "else" in
	let end_lbl = gen_temp "end_if" in
	if expr @
	test_bool @
	[ ICmp(Reg(EAX), true_const);
	  IJne(else_lbl);] @
	thenexpr @
	[ IJmp(end_lbl);
	  ILabel(else_lbl);] @
	elseexpr @
	[ ILabel(end_lbl)]
| EIf(EPrim(Greater, e1, e2) as ifexpr, thenexpr, elseexpr) ->
```



---



# Slide 2

![2](/home/shiro/Desktop/Translate/img/2.png)

```ocaml
| EPrim2(Plus, EApp(_, _) as e1, e2) ->
| EPrim2(Plus, ENum(n), e2) ->
| EPrim2(Plus, EPlus(_, _) as e1, e2) ->
| EPrim2(Plus, EPrim2(Greater, _, _ as e1, e2)) ->
```



---



# Slide 3

![3](/home/shiro/Desktop/Translate/img/3.png)



---



# Slide 4

![4](/home/shiro/Desktop/Translate/img/4.png)

```ocaml
let rec improve_instrs (is : instr list) : instr list = 
	match is with
		| (IMov(RegOffset(EBP, n), Reg(EAX))) ::
		  (IMov(Reg(EAX), RegOffset(EBP, n))) ::
		  	rest_instrs ->
	inprove_instrs(IMov(RegOffset(...) :: rest_instrs))
		...
		...
		| i :: rest ->
		  i :: (improve_instrs rest)
```

```assembly
f:
	mov ebp, esp
	sub esp, 12
	mov eax, 3
	mov [ebp - 4], eax
	mov eax, [ebp + 0]
	mov [ebp - 8], eax
	and eax, [ebp - 4]
	and eax, 1
	cmp eax, 1
	jne near error_non_int
	mov eax, [ebp - 8]
	and eax, 0xfffffffe
	add eax, [ebp - 4]
	jo near overflow_check
	mov [ebp - 4], eax
	mov eax, 3
	mov [ebp -12], eax
	and eax, [ebp - 8]
	and eax, 1
	cmp eax, 1
	jne near error_non_int
	mov eax, [ebp - 12]
	and eax, 0xfffffffe
	add eax, [ebp - 8]
	jo near overflow_check
	add esp, 16
	ret
```



---



# Slide 5

![5](/home/shiro/Desktop/Translate/img/5.png)

```ocaml
let rec improve_expr(e : expr):expr = 
	match e with
		| EPrim2(Plus, ENum(n1), ENum(n2)) -> ENum(n1 + n2)
		| ELet(X, ENum(n), body) ->
			replace x (ENum(n)) body
```



---



# Slide 6

![6](/home/shiro/Desktop/Translate/img/6.png)

```ocaml
let rec improve_expr(e : expr):expr = 
	match e with
		| EPrim2(Plus, ENum(n1), ENum(n2)) -> ENum(n1 + n2)
		| ELet(X, ENum(n), body) ->
			replace x (ENum(n)) body

and replace id e body =
	...
```



---

