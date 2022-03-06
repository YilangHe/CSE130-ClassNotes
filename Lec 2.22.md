# Lec 2/22 + Lec 2 /24

## Functions

```haskell
data Expr
	
	
	ELam Id Expr
	EApp Expr Expr 	
```

```haskell
eval env (e1 e2)         -- function call
eval env e1              -- >> VFun "x" eBody
eval env e2              -- >> v2
eval ["x" := v2] eBoay   


EApp eFun eArg -> eval ((x := v2) : env) eBody
									where
										VFun x eBody  = eval env eFun  -- >> eval a function and input is x and output is eBody
										v2            = eval env eArg

```

```haskell
let c = 1                -- > env
in   										 -- > c := 1 : env frozen env
	let inc = \x -> x + c   
	in 									-- inc:=<x, x+c, frozenenv>, c:=1, env input x, output x + 1
		let c = 100
		in 								-- > c:=100, inc:=<x, x+c, frozenenv>, c:=1, env		
			inc 10					-- eval (x:=10 froz) (x + c)
	  --inc c           -- eval (x:=100 froz) (x + c)
```

eval incr 10

1. Evaluate incr to get <froze, "x", x + c>

2. Evaluate 10 to get 10

3. Evaluate x + c in x:=10 : frozen

   ​               (body)  (param:=Arg)

### In general

1. Evaluate e1 to get <froze, param, body>
2. Evaluate e2 to get v2
3. Evaluate body in param:=v2 : frozenenv

