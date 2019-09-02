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

After that, you could add `-XX:+UnlockDiagnosticVMOptions -XX:+PrintAssembly ` in the Java Run Param to see the program's assebmle code.
One Example:
```
java -XX:+UnlockDiagnosticVMOptions -XX:+PrintAssembly -Xcomp -XX:CompileCommand=compileonly,*VolatileTest.main com.io.lzy.VolatileTest

```

There is already a prebuild hsdis-amd64 for OSX 64 in build/macosx-amd64/macosx-amd64.dylib and for linux in build/linux-amd64/hsdis-amd64.so and was build for/with Unbuntu 16.04
