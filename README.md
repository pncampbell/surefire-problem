# surefire-problem
project to investigate surefire forking crash

It might be Failsafe thats the actual problem ...

When using certain dependencies bundled into a fat jar (e.g. cxf-core) surefire/failsafe reports a crashed fork attempt.

    [INFO] BUILD FAILURE
    [INFO] ------------------------------------------------------------------------
    [INFO] Total time:  6.295 s
    [INFO] Finished at: 2022-04-12T16:36:03+01:00
    [INFO] ------------------------------------------------------------------------
    [ERROR] Failed to execute goal org.apache.maven.plugins:maven-failsafe-plugin:3.0.0-M5:verify (run-tests) on project artifact1: There are test failures.
    [ERROR] 
    [ERROR] Please refer to /home/paulcampbell/HMPO/surefire-problem/target/failsafe-reports for the individual test results.
    [ERROR] Please refer to dump files (if any exist) [date].dump, [date]-jvmRun[N].dump and [date].dumpstream.
    [ERROR] org.apache.maven.surefire.booter.SurefireBooterForkException: The forked VM terminated without properly saying goodbye. VM crash or System.exit called?
    [ERROR] Command was /bin/sh -c cd /home/paulcampbell/HMPO/surefire-problem && /usr/lib/jvm/java-11-openjdk-amd64/bin/java @/home/paulcampbell/HMPO/surefire-problem/target/surefire/surefireargs10961685891327111046 /home/paulcampbell/HMPO/surefire-problem/target/surefire 2022-04-12T16-36-01_469-jvmRun1 surefire6425831590946008469tmp surefire_01550115040662318041tmp

The jvmrun file contains this error:
        Created at 2022-04-12T16:36:02.807
        Corrupted STDOUT by directly writing to native stream in forked JVM 1. Stream 'Error occurred during initialization of boot layer'.
        Created at 2022-04-12T16:36:02.809
        Corrupted STDOUT by directly writing to native stream in forked JVM 1. Stream 'java.lang.module.ResolutionException: Module artifact1.BUILD contains package javax.transaction.xa, module java.transaction.xa exports package javax.transaction.xa to artifact1.BUILD'.

This problem seems to have been introduced in 3.0.0.-M5 since it will work as expected using failsafe 3.0.0.-M4.

The jar itself is runnable on command line without any problem in either case e.g.: 
java -jar target/artifact1-BUILD.jar

