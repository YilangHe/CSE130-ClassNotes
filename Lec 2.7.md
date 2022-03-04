# Lec 2/17

## Nano: Arithmetic 

```haskell
e ::= n 
		| e1 + e2
		| e1 - e2
		| e1 * e2
		
```

```haskell
type Value = Int
type Id = String
type Env = [(Id, Value)]

{-data Expr 
	= ENum Int 
	| EAdd Expr Expr
	| ESub Expr Expr 
	| EMul Expr Expr -}
	
data Expr 
	= ENum Int 
	| EBin Bop Expr Expr
	| EVar Id
	
data Bop = Add | Sub | Mul
	
expr :: Expr 
expr EBin Add (ENum 1) (ENum 2)

{- expr :: Expr 
expr = (ENum 4) `EAdd` (ENum 12) -}

{- eval :: Expr -> Value
eval e case e of 
	ENum n	-> n
	EAdd ex ex' -> eval e + eval e'
	ESub ex ex' -> eval e - eval e'
	EMul ex ex' -> eval e * eval e' -}
	
eval :: Env -> Expr -> Value
eval e case e of 
	EVar x  -> lookup x e 
	ENum n	-> n
	EBin o ex ex' -> evalOp o (eval env ex) (eval env ex')
	
lookup :: Id -> Env -> Value
lookup x env = case env of
	[] -> error "var not found"
	(y, v) : t -> if x == y then v else lookup x t
{-
Type of evalOp 
	evalOp :: Bop -> Value -> Value -> Value 
-}

evalOp :: Bop -> Value -> Value -> Value 
eval o v1 v2 = case o of 
	Add -> v1 + v2
	Sub -> v1 - v2
	Mul -> v1 * v2
	



```

## Nano: Variables

## Evalution in an Environment

```haskell
(eval env expr) => value
```







