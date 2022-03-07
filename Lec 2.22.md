# Lec 2/22 + Lec 2/24

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

### eval incr 10

1. Evaluate incr to get <froze, "x", x + c>

2. Evaluate 10 to get 10

3. Evaluate x + c in x:=10 : frozen

   ​               (body)  (param:=Arg)

### In general

1. Evaluate e1 to get <froze, param, body>
2. Evaluate e2 to get v2
3. Evaluate body in param:=v2 : frozenenv

```haskell
-- >>> let env1 = ["add" := VClosure "x" (ELam "y" (EBin Add (EVar "x")(EVar "y"))) []]
-- >>> let v_add10 = VClosure "y" (EBin Add (EVar "x")(Evar "y")) ["x:=10"]
-- >>> let env2 = "add10" := v_add10: env1
-- >>> let v_add20 = VClosure "y" (EBin Add (EVar "x")(EVar "y"))["x" := VInt 20]
-- >>> let env3 = "add20":= v_add20:env2
-- >>> eval env3 (EApp (EVar "add10")(ENum 100))
-- >>> eval env3 (EApp (EVar "add20")(ENum 1000))


ex_add =                -- []
	Elet "add" (Elam "x" (Elam "y" (EBin Add (EVar "x")(EVar "y")))) 
	(                     -- env1
		Elet "add10" (EApp (EVar "add")(ENum 10))
		(                   -- env2
			ELet "add 20" (EApp (EVar "add")(ENum 20)) 
			(                 -- env3
				EBin Add (EApp (EVar "add10")(ENum 100))(EApp (EVar "add20")(ENum 1000))	
			)
		)
	)
```



## Parsing 

```haskell
parse :: String -> Expr
```

Step 1: From String to tokens

Step 2: (Parsing) From Tokens to AST 