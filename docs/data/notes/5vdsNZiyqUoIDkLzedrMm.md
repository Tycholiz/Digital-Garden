
when coding a problem, you must first understand what you are coding before you understand how you are coding
- Put another way, you need to understand the underlying logic/business case that you are implementing with code. Understand the issue, understand how a resolution of that issue feels. The what is the important part. Once that's done, the how will flow naturally

### Robustness principle (Postel's law)
"be conservative (strict) in what you send, be liberal in what you accept"
- programs that send messages to other machines (or to other programs on the same machine) should conform completely to the specifications, but programs that receive messages should accept non-conformant input as long as the meaning is clear.