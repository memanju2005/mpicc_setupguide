



# MPI Setup, Compilation, and Running Guide for Linux and Windows

This guide will help you set up, compile, and run MPI C programs on **Linux (Ubuntu/Debian)** and **Windows (using Microsoft MPI)**.

## 1. Setting Up MPI

### For Linux (Ubuntu/Debian) — OpenMPI

1. Open a terminal.
2. Run:

```bash
sudo apt update
sudo apt install openmpi-bin openmpi-common libopenmpi-dev
```

3. Verify the MPI compiler is installed:

```bash
mpicc --version
```

4. More info: [OpenMPI Official Site](https://www.open-mpi.org/)

### For Windows — Microsoft MPI (MS-MPI)

1. Download the [MS-MPI installers from Microsoft](https://learn.microsoft.com/en-us/message-passing-interface/microsoft-mpi):
    - `msmpisetup.exe` (runtime)
    - `msmpisdk.msi` (developer SDK)
2. Install `msmpisetup.exe` first, then install `msmpisdk.msi`.
3. Usually, the installer adds required environment variables automatically.
If not, add this path to your system’s PATH environment variable:

```
C:\Program Files\Microsoft MPI\Bin\
```

4. Verify installation by opening Command Prompt and running:

```
mpicc --version
```


## 2. Write Your MPI Program

Create a C source file (e.g., `hello_mpi.c`) with content similar to:

```c
#include <mpi.h>
#include <stdio.h>

int main(int argc, char** argv) {
    MPI_Init(&argc, &argv);

    int rank;
    MPI_Comm_rank(MPI_COMM_WORLD, &rank);

    printf("Hello from process %d\n", rank);

    MPI_Finalize();
    return 0;
}
```

Save the file in your working directory.

## 3. Compile the MPI Program

- **Linux (Ubuntu/Debian):**

```bash
mpicc hello_mpi.c -o hello_mpi
```

- **Windows (MS-MPI):**
Open the **Developer Command Prompt for Visual Studio** or **MS-MPI Command Prompt**, navigate to your source directory, then run:

```
mpicc hello_mpi.c -o hello_mpi.exe
```


## 4. Run the MPI Program

Launch the program with your desired number of parallel processes.

- **Linux:**

```bash
mpiexec -np <number_of_processes> ./hello_mpi
# or
mpirun -np <number_of_processes> ./hello_mpi
```

- **Windows:**

```
mpiexec -np <number_of_processes> hello_mpi.exe
# or
mpirun -np <number_of_processes> hello_mpi.exe
```


**Example:** Run with 4 processes

```bash
mpiexec -np 4 ./hello_mpi      # Linux
mpiexec -np 4 hello_mpi.exe   # Windows
```


##  Important Note on Using More Processes than CPU Cores

- The `-np` option specifies how many MPI processes to launch.
- Your system may not have enough CPU cores (or "slots") to run all the requested processes simultaneously.
- If you receive an error like:

```
There are not enough slots available in the system to satisfy the <number_of_processes> slots that were requested
```

- **Solution:** Use the `--oversubscribe` option to allow running more MPI processes than available CPU cores (processes will share cores):
    - **Linux example:**

```bash
mpiexec --oversubscribe -np 8 ./hello_mpi
```

    - **Windows example:**

```
mpiexec --oversubscribe -np 8 hello_mpi.exe
```

- This is useful for testing and development but may reduce performance.

> **🚧 Engineer Alert! 🚧**
> If you run into any strange problems not covered here, don’t worry! Just roll up your sleeves and fix it yourself. You’re an engineer now — solving problems is what you do! 🛠️😎

