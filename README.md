# hsdis
Hotspot disassembler extracted from openjdk.

# Precondition on linux
To build hsdis on linux you need to make sure you have the standard build tools
```
apt install build-essential
```

# Tool Versions
GCC version 7+ should be able to compile binutils version 2.29+ with no errors


# Usage
```
git clone https://github.com/ShuyangLiu/hsdis
cd hsdis
tar -zxvf binutils-2.32.tar.gz
make BINUTILS=binutils-2.32 ARCH=arm
```
And then copy hsdis build file to the target folder in JDK.

### OSX
```
sudo cp build/macosx-amd64/hsdis-amd64.dylib /Library/Java/JavaVirtualMachines/jdk1.8.0_131.jdk/Contents/Home/jre/lib/server/
```
### Linux
```
sudo cp build/linux-amd64/hsdis-amd64.so /usr/lib/jvm/java-8-oracle/jre/lib/amd64/server/
```

Alternatively, if you don't have sudo access, you can put the file on JVM's search path:
```
export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:path/to/hsdis/build/linux-amd64
```

After that, you could add `-XX:+UnlockDiagnosticVMOptions -XX:+PrintAssembly ` in the Java Run Param to see the program's assebmle code.
For Example, for the following code:

```java

public class Test {

  public static void main(String [] args) {
    Test hello = new Test();
    for(int i = 0; i <= 10_000_000; i++) {
      hello.hello(i);
    }
  }

  private void hello(int i) {
    if (i % 1_000_000 == 0) {
      System.out.println("Hello, " + i);
    }
  }
}

```

We can run it using the following command:

```
javac Test.java
java -XX:+UnlockDiagnosticVMOptions -XX:+PrintAssembly \
 -XX:+TraceClassLoading -XX:+LogCompilation \
 -Xcomp -XX:CompileCommand=compileonly,*Test.hello \
 Test
```

There is already a prebuild hsdis-amd64 for OSX 64 in build/macosx-amd64/macosx-amd64.dylib and for linux in build/linux-amd64/hsdis-amd64.so and was build for/with Unbuntu 16.04
