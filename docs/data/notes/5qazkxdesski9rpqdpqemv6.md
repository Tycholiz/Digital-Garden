
attributes written in JSX become keys of JavaScript objects

Why do multiple JSX tags need to be wrapped? 
- Because JSX is transformed into plain JavaScript objects. You can’t return two objects from a function without wrapping them into an array. This explains why you also can’t return two JSX tags without wrapping them into another tag or a Fragment.