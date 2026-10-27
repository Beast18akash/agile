# ASD & DevOps --- Experiment 3

## Set Up and Execute a Continuous Integration Pipeline Using Jenkins

**Environment:** Windows PC\
**Tools:** Jenkins, Java JDK 21, Python 3, web browser, Command Prompt\
**Aim:** To set up and execute a Continuous Integration pipeline using
Jenkins.

> This guide follows the five examples listed in the Experiment 3
> procedure in the lab manual. Complete each example and keep
> screenshots of the successful build and Console Output for your
> journal.

------------------------------------------------------------------------

## 1. Before You Start

On the college PC, check whether Java, Python, and Git are installed.
Open **Command Prompt** and run:

``` cmd
java -version
python --version
```

-   Jenkins 2.580.1 requires Java 21 or Java 25 to run. If the PC has
    only Java 17, install a supported Java version first.
-   Python is required for Example 5.
-   If a command is not recognized, tell the lab instructor or
    install/configure that tool if you have permission on the college
    PC.

**Important:** Do not install software or change service settings on a
college-managed PC without permission.

------------------------------------------------------------------------

## 2. Install and Start Jenkins

1.  Open the official Jenkins website:
    <https://www.jenkins.io/download/>.

2.  Download the **Windows installer (.msi)**.

3.  Run the installer.

4.  Follow the setup wizard:

    -   For service logon, use the option permitted by your college PC
        administrator. If you do not have permission to configure a
        service account, ask the lab instructor.
    -   Keep port **8080** if the port test succeeds. If it is already
        occupied, ask the instructor which port to use.
    -   At the Java home screen, select the installed **Java 21 or Java
        25 JDK/JRE directory**. Do not select a Java 17 directory for
        Jenkins 2.580.1.
    -   Continue the installation and finish the wizard.

5.  Open a browser and visit:

    `http://localhost:8080`

### Unlock Jenkins

1.  If the **Unlock Jenkins** screen appears, open File Explorer.

2.  Paste this path into its address bar:

    `C:\ProgramData\Jenkins\.jenkins\secrets`

3.  Open `initialAdminPassword` with Notepad and copy the password.

4.  Paste it into the browser and click **Continue**.

If the file is not accessible, ask the lab instructor or administrator;
do not try to bypass Windows permissions.

### Install plugins and create an administrator

1.  Select **Install suggested plugins** and wait for installation to
    finish.
2.  If prompted, create the first administrator account. Record the
    username and password securely for your own use.
3.  Complete the setup wizard and open the Jenkins dashboard.

**If plugin installation fails:** take a screenshot of the full error
and ask the instructor to check internet access, Jenkins service status,
and the Jenkins logs. Do not keep retrying indefinitely or delete
Jenkins files.

------------------------------------------------------------------------

## 3. Create a Folder for the Example Programs

Create a folder named `jenkinsprojects` on the `C:` drive, if you have
permission.

In Command Prompt:

``` cmd
mkdir C:\jenkinsprojects
cd /d C:\jenkinsprojects
```

If the folder already exists, just use `cd /d C:\jenkinsprojects`.

------------------------------------------------------------------------

# Example 1 --- Run a Simple Windows Command in Jenkins

### A. Create the Jenkins job

1.  From the Jenkins dashboard, click **New Item**.
2.  Enter the name `Example1`.
3.  Select **Freestyle project**.
4.  Click **OK**.
5.  Scroll to **Build Steps**.
6.  Click **Add build step** → **Execute Windows batch command**.

### B. Enter the command

Paste this into the command box:

``` bat
echo Hello from Jenkins
echo Experiment 3 - Example 1
echo Simple Windows batch command executed successfully
```

7.  Click **Save**.
8.  Click **Build Now**.
9.  In **Build History**, click the latest build number (for example,
    `#1`).
10. Click **Console Output**.

### Expected result

The console should display:

``` text
Hello from Jenkins
Experiment 3 - Example 1
Simple Windows batch command executed successfully
```

Take a screenshot of the successful build and Console Output.

------------------------------------------------------------------------

# Example 2 --- Compile and Run a Java Program in Jenkins

### A. Create `HelloWorld.java`

Open Notepad or VS Code and save the following file as:

`C:\jenkinsprojects\HelloWorld.java`

``` java
public class HelloWorld {
    public static void main(String[] args) {
        System.out.println("Hello from Java running in Jenkins!");
        System.out.println("Experiment 3 - Example 2");
    }
}
```

Make sure the filename is exactly `HelloWorld.java`, not
`HelloWorld.java.txt`.

### B. Test Java locally

In Command Prompt:

``` cmd
cd /d C:\jenkinsprojects
javac HelloWorld.java
java HelloWorld
```

Expected output:

``` text
Hello from Java running in Jenkins!
Experiment 3 - Example 2
```

If `javac` is not recognized, the JDK's `bin` folder may not be on PATH.
Ask the instructor to configure it or use the full path to `javac.exe`
and `java.exe`.

### C. Create the Jenkins job

1.  In Jenkins, click **New Item**.
2.  Enter `Example2`.
3.  Select **Freestyle project** → **OK**.
4.  Under **Build Steps**, select **Add build step** → **Execute Windows
    batch command**.
5.  Enter:

``` bat
cd /d C:\jenkinsprojects
javac HelloWorld.java
if errorlevel 1 exit /b 1
java HelloWorld
```

6.  Click **Save** → **Build Now**.
7.  Open the build number → **Console Output**.

### Expected result

The console should show the Java program's two output lines and a
successful build.

Take a screenshot of the successful build and Console Output.

------------------------------------------------------------------------

# Example 3 --- Parameterized Build (Name and City)

This job asks for a name and a city at build time, then prints the
values.

### A. Create the job

1.  Click **New Item**.
2.  Enter `Example3`.
3.  Select **Freestyle project** → **OK**.
4.  In the **General** section, select **This project is
    parameterized**.

### B. Add the String Parameter

1.  Click **Add Parameter** → **String Parameter**.
2.  Set **Name** to `UserName`.
3.  Set a default value such as `Akash` (optional).

### C. Add the Choice Parameter

1.  Click **Add Parameter** → **Choice Parameter**.
2.  Set **Name** to `City`.
3.  Enter one city per line, for example:

``` text
Mumbai
Pune
Nashik
```

### D. Add the build command

1.  Under **Build Steps**, click **Add build step** → **Execute Windows
    batch command**.
2.  Enter:

``` bat
echo User Name: %UserName%
echo City: %City%
echo Parameterized Jenkins build completed successfully
```

3.  Click **Save**.
4.  On the job page, click **Build with Parameters**.
5.  Enter a name and select a city.
6.  Click **Build**.
7.  Open the build number → **Console Output**.

### Expected result

For example, if you enter `Akash` and select `Mumbai`, the console
should show:

``` text
User Name: Akash
City: Mumbai
Parameterized Jenkins build completed successfully
```

Take a screenshot of the parameter form and the successful Console
Output.

------------------------------------------------------------------------

# Example 4 --- Java Multiplication Table Using a Build Parameter

### A. Create `MultiplicationTable.java`

Save this file as:

`C:\jenkinsprojects\MultiplicationTable.java`

``` java
public class MultiplicationTable {
    public static void main(String[] args) {
        if (args.length == 0) {
            System.out.println("Please provide a number.");
            System.exit(1);
        }

        int num = Integer.parseInt(args[0]);

        System.out.println("Multiplication table of " + num + ":");
        for (int i = 1; i <= 10; i++) {
            System.out.println(num + " x " + i + " = " + (num * i));
        }
    }
}
```

### B. Test it locally

In Command Prompt:

``` cmd
cd /d C:\jenkinsprojects
javac MultiplicationTable.java
java MultiplicationTable 5
```

Expected output includes `5 x 1 = 5` through `5 x 10 = 50`.

### C. Create the Jenkins job

1.  Click **New Item**.
2.  Enter `Example4`.
3.  Select **Freestyle project** → **OK**.
4.  Select **This project is parameterized**.
5.  Click **Add Parameter** → **String Parameter**.
6.  Set **Name** to `num` and **Default Value** to `5`.
7.  Under **Build Steps**, choose **Add build step** → **Execute Windows
    batch command**.
8.  Enter:

``` bat
cd /d C:\jenkinsprojects
javac MultiplicationTable.java
if errorlevel 1 exit /b 1
java MultiplicationTable %num%
```

9.  Click **Save**.
10. Click **Build with Parameters**, enter `5` (or another integer) for
    `num`, then click **Build**.
11. Open the build number → **Console Output**.

### Expected result

The console should display the multiplication table for the number
supplied. Take a screenshot of the successful build and Console Output.

------------------------------------------------------------------------

# Example 5 --- Run a Python Program Using a Parameter

### A. Create `multiply.py`

Save this file as:

`C:\jenkinsprojects\multiply.py`

``` python
import sys

if len(sys.argv) < 2:
    print("Please provide a number.")
    sys.exit(1)

num = int(sys.argv[1])

print(f"Multiplication table of {num}:")
for i in range(1, 11):
    print(f"{num} x {i} = {num * i}")
```

### B. Test it locally

In Command Prompt:

``` cmd
cd /d C:\jenkinsprojects
python multiply.py 5
```

If `python` is not recognized, try:

``` cmd
py multiply.py 5
```

Use whichever command works on that PC in the Jenkins job below.

### C. Create the Jenkins job

1.  Click **New Item**.
2.  Enter `Example5`.
3.  Select **Freestyle project** → **OK**.
4.  Select **This project is parameterized**.
5.  Click **Add Parameter** → **String Parameter**.
6.  Set **Name** to `num` and **Default Value** to `5`.
7.  Under **Build Steps**, choose **Add build step** → **Execute Windows
    batch command**.
8.  If `python multiply.py 5` worked locally, enter:

``` bat
cd /d C:\jenkinsprojects
python multiply.py %num%
```

If only `py multiply.py 5` worked locally, use:

``` bat
cd /d C:\jenkinsprojects
py multiply.py %num%
```

9.  Click **Save**.
10. Click **Build with Parameters**, enter `5` for `num`, then click
    **Build**.
11. Open the build number → **Console Output**.

### Expected result

The console should display the multiplication table for the number
entered. Take a screenshot of the successful build and Console Output.

------------------------------------------------------------------------

## 4. Final Verification Checklist

Before you finish, verify each applicable item:

-   [ ] Jenkins dashboard opens at `http://localhost:8080`.
-   [ ] `Example1` runs a Windows batch command successfully.
-   [ ] `Example2` compiles and runs `HelloWorld.java`.
-   [ ] `Example3` accepts a name and city through **Build with
    Parameters**.
-   [ ] `Example4` prints a Java multiplication table using parameter
    `num`.
-   [ ] `Example5` prints a Python multiplication table using parameter
    `num`.
-   [ ] Each completed job has a successful build and a Console Output
    screenshot.
-   [ ] Screenshots and outputs are saved for the practical journal.

Do not mark a job complete until its build succeeds and its output is
verified.

------------------------------------------------------------------------

## 5. Conclusion

This experiment demonstrates how Jenkins Freestyle Projects can automate
Windows commands and the execution of Java and Python programs.
Parameterized builds allow users to provide runtime values, and the
Console Output is used to verify each build.

## Quick Troubleshooting

-   **`localhost:8080` does not open:** Check that the Jenkins service
    is running and that the configured port is 8080.
-   **Jenkins plugin installation fails:** Check internet connectivity
    and ask the instructor to inspect Jenkins logs. Do not retry
    repeatedly without checking the error.
-   **`javac` is not recognized:** Confirm that a JDK is installed and
    its `bin` directory is configured in PATH.
-   **`python` is not recognized:** Test `py --version`; use `py` in the
    Jenkins batch command if it works.
-   **A build fails:** Open the build number → **Console Output**. Read
    the first actual error, fix that issue, and build again.
-   **Access denied to a folder or service:** Ask the lab instructor or
    administrator; do not bypass PC permissions.

## Official Reference

-   Jenkins downloads: <https://www.jenkins.io/download/>
