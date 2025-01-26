Since a lot of people ask questions about BIMserver and configuring it's Heap Size, this page hopes to clarify some details.

Java, unlike most native software, needs to know the maximum amount of memory it can use upfront. You have to specify this maximum amount of memory (Heap Size) to the Java Virtual Machine (JVM) using the -Xmx argument. Here’s how to do it:

## Setting the Heap Size

For 4GB of memory:

```
-Xmx4g
```

For 16GB of memory:

```
-Xmx16g
```

## Considerations for choosing the heap size

Java will not necessarily use all the memory that is given, `-Xmx` specifies just a maximum amount it can use.
The amount should cover the BIMserver requirements, but not exceed the available resources.

### BIMserver Requirements:

BIMserver needs at least 1GB.
Depending on usage and plugins, BIMserver might require up to 100GB.
See [Memory usage](Memory-usage.md).

### External Limitations:

The available and usable amount of memory is also limited by various factors, namely your machine's physical resources, the operating system and the JVM bitness.

1. You cannot allocate more memory than the physical (or virtual) memory of your machine. It has to be less then that because other software (including the OS) will also use memory. Depending on what OS/software is installed, keep at least 1GB free for the other software.

2. Your computer and operating system can have a 32-bit or 64-bit architecture. With 32-bit OS, the maximum physical memory that can be addressed is limited to 4GB. With 64-bit there are no relevant limitations for memory allocation.

3. The JVM cannot use the full physical and addressable memory due to the reasons under 1. A 32-bit JVM on a 32-bit OS usually can use up to approximately 1300MB of memory, which is not much! Thus, if you are on 64-bit OS, ensure you are using a 64-bit JVM to avoid memory limitations. Using a 32-bit JVM on a 64-bit OS is not recommended for running a BIMserver.

## Tomcat user

In order to change the memory allocation in Tomcat, please follow this step:

```bash
user@local:~$ mkdir /opt/tomcat/bin/setenv.sh     # create setenv.sh manually
user@local:~$ vim /opt/tomcat/bin/setenv.sh       # edit to set memory
```

content of `/opt/tomcat/bin/setenv.sh `:

```bash
CATALINA_OPTS="-Xmx{new allocated memory}"
```
