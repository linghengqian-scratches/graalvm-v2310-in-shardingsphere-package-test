# graalvm-v2310-in-shardingsphere-package-test

- For https://github.com/oracle/graal/issues/7500.
- Also for https://github.com/apache/shardingsphere/pull/28609 and https://github.com/apache/shardingsphere/issues/21347 .
- On the new Ubuntu 22.04.3 instance, execute the following command:
```bash
sudo apt install unzip zip curl sed -y
curl -s "https://get.sdkman.io" | bash
source "$HOME/.sdkman/bin/sdkman-init.sh"
sdk install java 21-graalce
sdk use java 21-graalce

git clone -b update-graalvm-jdk21 https://github.com/linghengqian/shardingsphere.git
cd ./shardingsphere/
git reset --hard f168011d597c4da21c4b52bd35f9ef9af1b45b96
./mvnw -am -pl distribution/proxy-native -T1C -e -Prelease.native -DskipTests clean package
```
- You'll notice a javac-related Error Log. This Error Log is logged in https://github.com/linghengqian/graalvm-v2310-in-shardingsphere-package-test/actions/runs/6326886950/job/17181459201 .

```bash
========================================================================================================================
GraalVM Native Image: Generating 'apache-shardingsphere-proxy-native' (executable)...
========================================================================================================================
For detailed information and explanations on the build output, visit:
https://github.com/oracle/graal/blob/master/docs/reference-manual/native-image/BuildOutput.md
------------------------------------------------------------------------------------------------------------------------
[1/8] Initializing...                                                                                   (20.6s @ 0.28GB)
 Java version: 21+35, vendor version: GraalVM CE 21+35.1
 Graal compiler: optimization level: 2, target machine: x86-64-v3
 C compiler: gcc (linux, x86_64, 11.4.0)
 Garbage collector: Serial GC (max heap size: 80% of RAM)
 2 user-specific feature(s):
 - com.oracle.svm.polyglot.groovy.GroovyIndyInterfaceFeature
 - com.oracle.svm.thirdparty.gson.GsonFeature
------------------------------------------------------------------------------------------------------------------------
Build resources:
 - 5.11GB of memory (75.6% of 6.76GB system memory, determined at start)
 - 2 thread(s) (100.0% of 2 available processor(s), determined at start)
[2/8] Performing analysis...  []                                                                       (174.6s @ 2.19GB)
   28,875 reachable types   (77.2% of   37,420 total)
   47,147 reachable fields  (52.3% of   90,097 total)
  139,427 reachable methods (54.9% of  254,163 total)
    8,604 types,   300 fields, and 8,731 methods registered for reflection

------------------------------------------------------------------------------------------------------------------------
Error: java.util.concurrent.ExecutionException: java.lang.ClassCastException: class jdk.vm.ci.meta.NullConstant cannot be cast to class org.graalvm.compiler.core.common.type.TypedConstant (jdk.vm.ci.meta.NullConstant is in module jdk.internal.vm.ci of loader 'bootstrap'; org.graalvm.compiler.core.common.type.TypedConstant is in module jdk.internal.vm.compiler of loader 'platform')
                       61.0s (31.0% of total time) in 228 GCs | Peak RSS: 4.42GB | CPU load: 1.92
========================================================================================================================
Finished generating 'apache-shardingsphere-proxy-native' in 3m 15s.
```
