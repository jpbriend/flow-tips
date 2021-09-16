
## Prerequisites
* have a Procedure in the Pipeline which generates an Output Parameter

In this example, the task of the procedure will be named `myTask1` and the Output Parameter will be named `myOutputParameter`.

## Syntax to access the Output Parameter from another task in the same Stage

```
/myStageRuntime/tasks/myTask1/job/outputParameters/myOutputParameter
```
