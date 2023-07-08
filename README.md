### Задание
Компиляция и интерпретация кода
Создать docker-контейнер для формирования полной документации по проекту

```
FROM bellsoft/liberica-openjdk-alpine AS builder

# Copy the source code to the container
COPY ./src /usr/src/app/src

# Set the working directory
WORKDIR /usr/src/app

# Compile the source code
# RUN javac -sourcepath src/main/java -d out src/main/java/org/example/Main.java
RUN javac -sourcepath ./src/main/java -d out src/main/java/rust/sample/Main.java
# RUN javac -sourcepath .\src\main\java\ -d out src/main/java/sample/Main.java

# Generate Javadoc
RUN javadoc -d docs -sourcepath ./src/main/java -cp ./out -subpackages rust
# RUN javadoc -d docs -sourcepath src/main/java -subpackages org

# Build stage
FROM nginx:alpine

# Copy the generated Javadoc documentation
COPY --from=builder /usr/src/app/docs /usr/share/nginx/html

# Expose the documentation port
EXPOSE 80

# Start the web server to serve the Javadoc documentation
CMD ["nginx", "-g", "daemon off;"]

```
```
root@linux-gb:/home/rust/jcore# docker build -t jc .
Sending build context to Docker daemon  378.9kB
Step 1/9 : FROM bellsoft/liberica-openjdk-alpine AS builder
latest: Pulling from bellsoft/liberica-openjdk-alpine
8a49fdb3b6a5: Pull complete
bd77b4301f50: Pull complete
702c8f9bd880: Pull complete
Digest: sha256:9c009d70cd04bf82e40ceb877d3092a8c0c21308423e3da622bd2d5cd99341a6
Status: Downloaded newer image for bellsoft/liberica-openjdk-alpine:latest
 ---> c36fec93a7f3
Step 2/9 : COPY ./src /usr/src/app/src
 ---> 3fac96104a2d
Step 3/9 : WORKDIR /usr/src/app
 ---> Running in c12137ce2588
Removing intermediate container c12137ce2588
 ---> 7d08a1aedabf
Step 4/9 : RUN javac -sourcepath ./src/main/java -d out src/main/java/rust/sample/Main.java
 ---> Running in d435416cfe2a
Removing intermediate container d435416cfe2a
 ---> 2d4b1a078932
Step 5/9 : RUN javadoc -d docs -sourcepath ./src/main/java -cp ./out -subpackages rust
 ---> Running in 72b986456a74
Loading source files for package rust...
Constructing Javadoc information...
Creating destination directory: "docs/"
Building index for all the packages and classes...
Standard Doclet version 20.0.1+10
Building tree for all the packages and classes...
Generating docs/rust/regular/Decorator.html...
./src/main/java/rust/regular/Decorator.java:6: warning: use of default constructor, which does not provide a comment
public class Decorator {
       ^
Generating docs/rust/sample/Main.html...
./src/main/java/rust/sample/Main.java:17: warning: use of default constructor, which does not provide a comment
public class Main {
       ^
Generating docs/rust/regular/OtherClass.html...
./src/main/java/rust/regular/OtherClass.java:9: warning: no main description
     * @param a
       ^
./src/main/java/rust/regular/OtherClass.java:9: warning: no description for @param
     * @param a
       ^
./src/main/java/rust/regular/OtherClass.java:10: warning: no description for @param
     * @param b
       ^
./src/main/java/rust/regular/OtherClass.java:17: warning: no main description
     * @param a
       ^
./src/main/java/rust/regular/OtherClass.java:17: warning: no description for @param
     * @param a
       ^
./src/main/java/rust/regular/OtherClass.java:18: warning: no description for @param
     * @param b
       ^
./src/main/java/rust/regular/OtherClass.java:26: warning: no main description
     * @param a
       ^
./src/main/java/rust/regular/OtherClass.java:26: warning: no description for @param
     * @param a
       ^
./src/main/java/rust/regular/OtherClass.java:27: warning: no description for @param
     * @param b
       ^
./src/main/java/rust/regular/OtherClass.java:35: warning: no main description
     * @param a
       ^
./src/main/java/rust/regular/OtherClass.java:35: warning: no description for @param
     * @param a
       ^
./src/main/java/rust/regular/OtherClass.java:36: warning: no description for @param
     * @param b
       ^
./src/main/java/rust/regular/OtherClass.java:7: warning: use of default constructor, which does not provide a comment
public class OtherClass {
       ^
Generating docs/rust/regular/package-summary.html...
Generating docs/rust/regular/package-tree.html...
Generating docs/rust/sample/package-summary.html...
Generating docs/rust/sample/package-tree.html...
Generating docs/overview-tree.html...
Generating docs/index.html...
Building index for all classes...
Generating docs/allclasses-index.html...
Generating docs/allpackages-index.html...
Generating docs/index-all.html...
Generating docs/search.html...
Generating docs/overview-summary.html...
Generating docs/help-doc.html...
15 warnings
Removing intermediate container 72b986456a74
 ---> b22a188dc670
Step 6/9 : FROM nginx:alpine
alpine: Pulling from library/nginx
4db1b89c0bd1: Pull complete
bd338968799f: Pull complete
6a107772494d: Pull complete
9f05b0cc5f6e: Pull complete
4c5efdb87c4a: Pull complete
c8794a7158bf: Pull complete
8de2a93581dc: Pull complete
768e67c521a9: Pull complete
Digest: sha256:2d194184b067db3598771b4cf326cfe6ad5051937ba1132b8b7d4b0184e0d0a6
Status: Downloaded newer image for nginx:alpine
 ---> 4937520ae206
Step 7/9 : COPY --from=builder /usr/src/app/docs /usr/share/nginx/html
 ---> 209d91fcf8f6
Step 8/9 : EXPOSE 80
 ---> Running in 9fe640b5b639
Removing intermediate container 9fe640b5b639
 ---> d790d342f561
Step 9/9 : CMD ["nginx", "-g", "daemon off;"]
 ---> Running in 4f4a7bfa8946
Removing intermediate container 4f4a7bfa8946
 ---> 794afef4116c
Successfully built 794afef4116c
Successfully tagged jc:latest
```


![javadoc](https://github.com/Ledsager/hw1javacore1/blob/main/javadoc.jpg)
