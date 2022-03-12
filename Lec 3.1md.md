## TypeClasses
```haskell
data Env k v 
	= Def v
	| Bind k v ( Env k v )
	deriving (Show)
	

get :: Eq => k -> Env k v -> v --k should implement the Eq
get key env = case env of 
	Def  v -> v
	Bind k v env' -> if key == k then v else get key env'
	
	
data Tree k v 
 	= Leaf 
  | Node k v (Tree k v) (Tree k v)

--               k -> v
--       LTree              RTree
```

### 3.3 Content
```haskell
{-
(+)
(<=)
( == )
show
-}

foo = ( + )

emp :: v -> Env k v 
emp v = Def v

set :: k -> v -> Env k v -> Env k v
set key val table = Bind key val table

get :: Eq k => k -> Env k v -> v -- PA we need to implement the k in increasing order
get key t = case t of
	Def v -> v 
	Bind k v env -> if key == k then v else get key env

menu :: Env String Int
menu = set "sandwich" 10 (set "burrito" 11 (set "burger" 15 (emp 0)))

menu_descr = set "sandwich" (["bacon", "lettuce", "tomato"], 10) (emp ([], 0) )

get "coffee" menu
```

```haskell
read :: Read a => String -> a

quiz :: Int 
quiz = read "2"


quiz' = read "2" :: Int
```

## JSON Datatype

```haskell
data JVal
   = JStr String
   |JNum Double
   |JBool Bool
   |JObj  [(String, JVal)]
   |JArr  [JVal]
   deriving (Eq, Ord, Show)
   
class JSON a where
	jval :: a -> Jval
	
Instance JSON Int where
	jval n = JNum (fromIntegral n)

Instance JSON Double where
	jval n = JNum n
	
Instance JSON String where
	jval s = JStr s
	
Instance JSON a => JSON [a] where
	jval xs = JArr [ JStr x | x <- xs ] -- (x <- xs) for each x in xs
	
Instance (JSON a, JSON b) => JSON (a, b) where
	jval (x, y) = JObj [("fst", jval x), ("snd", jval y)]
	
Instance JSON a => JSON ( Env String a ) where 
	jval env = JObj [(k, jval v) | (k, v) <- elems env]
   
-- jval :: Env String a -> JVal
-- jval env = JObj[ (k, JNum (fromIntegral v))  | (k , v) <- elems env] 

elems :: Env String a -> [(String, a)]
elems (Def v) = [("default", v)]
elems (Bind k v rest) = (k, v)   : elems rest
```



## Monads

```haskell
mapEnv :: (a -> b) -> Env k a -> Env k b 
mapEnv f env = case env of 
	Def x0 -> Def (f x0)
	Bind k v env' -> Bind k (f v) (mapEnv f env')
	
convert_menu :: Env String ( [String], Int ) -> Env String Int
convert_menu env = case env of 
	Def x0 -> Def (snd x0)
	Bind k v env' -> Bind k (snd v) (convert_menu env')

bump' :: Env String Int -> Env String Int
bump' = mapEnv (+ 1)  

bump :: Env String Int -> Env String Int
bump env = case env of 
	Def n -> Def (n + 1)
	Bind s n env' -> Bind s (n + 1) (bump env')

```

```haskell
mapList :: (a -> b) -> List a -> List b

class Mappable t where
	map :: (a -> b) -> t a -> t b
```





