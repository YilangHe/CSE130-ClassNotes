```haskell
Top  : ID '=' Expr                 { $3 }
     | Expr                        { $1 }

Expr : TNUM                        { EInt $1 }
     | '(' Expr ')'                { $2 }
     | true                        { EBool True }
     | false                       { EBool False }
     | ID                          { EVar $1 }
     | let ID '=' Expr in Expr     { ELet $2 $4 $6 }
     | let ID IDs '=' Expr in Expr { ELet $2 (mkLam $3 $5) $7 }
     | '\\' ID '->' Expr           { ELam $2 $4 }
     | if Expr then Expr else Expr { EIf $2 $4 $6 }
     | Expr '+' Expr               { EBin Plus  $1 $3}
     | Expr '-' Expr               { EBin Minus $1 $3}
     | Expr '*' Expr               { EBin Mul $1 $3}
     | Expr '<' Expr               { EBin Lt  $1 $3}
     | Expr '<=' Expr              { EBin Le  $1 $3}
     | Expr '==' Expr              { EBin Eq  $1 $3}
     | Expr '/=' Expr              { EBin Ne  $1 $3}
     | Expr '||' Expr              { EBin Or  $1 $3}
     | Expr '&&' Expr              { EBin And  $1 $3}
     | Expr  Expr                  { EApp $1 $2 }
     | '[' ']'                     { ENil }
     | Expr ':' Expr               { EBin Cons $1 $3 }
     | Expr ',' Expr               { EBin Cons $1 $3 }
     | '[' Expr ']'                { $2 }

ExprL : ExprL ExprR                { EApp $1 $2}  
      | '\\' ID '->' Expr          { ELam $2 $4 }

ExprR : TNUM                        { EInt $1 }
      | true                        { EBool True }
      | false                       { EBool False }
      | ID                          { EVar $1 }
      | '[' ']'                     { ENil }
      | '[' Expr ']'                { $2 }
      | '(' Expr ')'                { $2 }

     
IDs  : ID                          { [$1] }
     | ID IDs                      { $1 : $2 }


```

