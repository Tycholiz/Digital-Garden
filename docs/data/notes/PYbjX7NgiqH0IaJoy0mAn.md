
A `proc` object holds a code block to be executed, and can be stored in a variable.
- it is an instance of the `Proc` class.

Proc is short for procedure.

When calling a proc, the program yields control to the code block in the proc. So, if the proc returns, the current scope returns. If a proc is called inside a function and calls `return`, the outer function immediately returns as well.

When using parameters prefixed with ampersands, passing a block to a method results in a proc in the method’s context. Procs behave like blocks, but they can be stored in a variable.

To create a proc, you call Proc.new and pass it a block:
```rb
def run_proc_with_random_number(proc)
  proc.call(rand)
end

proc = Proc.new { |n| puts "#{n}!" }
run_proc_with_random_number(proc) # 0.8417555636545129!
```

Since a proc can be stored in a variable, it can also be passed to a method just like a normal argument.

Instead of creating a proc and passing that to the method, you can use Ruby’s ampersand parameter syntax and use a block instead:
- this will convert a passed block to a proc object and store it in a variable in the method scope.
```rb
def run_proc_with_random_number(&proc)
  proc.call(random)
end

run_proc_with_random_number { |n| puts "#{n}!" }
```

note: While it’s useful to have the proc in the method in some situations, the conversion of a block to a proc produces a performance hit. Whenever possible, use implicit blocks instead.
