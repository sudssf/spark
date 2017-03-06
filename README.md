# WholeStageCodeGen Debug

WholeStageCodeGen/Catalyst dynamically generates java code while executing SQL queries. This introduces additional complexity when the developer tries to optimize/debug the execution flow. If any of the function in dynamically generated code pops up as hot(eg : processNext()), there is no code to refer back.

This branch adds required debug code to help in this situtation. This will dump all dynamically generated code to /tmp/catalyst-output directory. The file name would be in the following format.

q10_jobid_6_stageid_3_shuffleTask_6_execid_1.java     

* q10 - Application Name
* 6   - Job Id
* 3   - Stage id
* 1   - Executor Id
* shuffle/Result Task - TaskType

The developer needs to refer SparkUI for job & stage id map, and then he/she can refer the dynamically generated code for that stage. 

To modify the dynamically generated code, developer need to modify the generated java file in previous run from catalyst-output directory and copy it to /tmp/catalyst-input directory. If WholeStageCodeGen finds a java file for the given job and stage id, then it will use that source code instead of generating a new code. This can be used for quick debugging and making some experiment.

