This page describes how to set an output parameter and how to use this parameter later in another procedure
## Prerequisites
* Use procedures!

In this example, a first procedure `subproc1` produces an output parameter. This procedure is used as a sub-procedure in a step num. 1. A second step consumes this output parameter.

## Set an output parameter

* Declare an output parameter in your procedure.
* Fill this parameter during the execution of the procedure via:
```
use strict;
use ElectricCommander;
my $ec = new ElectricCommander();
...
$ec->setOutputParameter("extracted_value", "myValue");
```
In this example,  `extracted_value` is the name of the output parameter and `myValue` is the value of this parameter.

## Consume the output parameter

Let's say the the previous procedure is called in a Procedure Step called `subproc1`.
You can access the output parameter this way:

```
$[/myJob/jobSteps[subproc1]/outputParameters/extracted_value]
```
