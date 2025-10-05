# Linux Kernel Debug

- **Kernel Oops:**
  - A **kernel oops** is a way for the Linux kernel to communicate to the user that an error has occurred.  
  - When the kernel detects a problem, it kills the offending process and prints an *oops message* in the log, including the current system status and stack trace.  
  - Different kinds of errors can generate a kernel oops, such as illegal memory access or execution of invalid instructions.  

  - **Tools:**
    - `addr2line -f -e vmlinux <address shown in PC register>`
    - Alternatively, use **gdb**:  
      ```bash
      gdb vmlinux
      (gdb) list *(function_name + offset)
      ```

---

- **PSTORE** (If you do not have access, how to debug the kernel):
  - **Pstore** is a generic kernel framework for persistent data storage and can be enabled with the `CONFIG_PSTORE` option.
  - It allows storing crash logs or kernel oops messages persistently across reboots, which helps in post-mortem debugging when:
    - The system crashes before logs are flushed to disk.
    - You do not have access to the serial console.

---

- **KDUMP:**
  - **Kdump** uses `kexec` to quickly boot into a dump-capture kernel whenever a crash occurs.  
  - It generates the **vmcore** file when the kernel crashes.  
  - You can then analyze the dump using **GDB**:  
    ```bash
    gdb vmlinux -c vmcore -tui
    ```

---

- **KGDB:**
  - Compile the kernel with **KGDB** support.  
  - Configure the Linux kernel on the target to run in debug mode.  
    - Runtime configuration:
      ```bash
      echo ttyS0 > /sys/module/kgdboc/parameters/kgdboc
      echo g > /proc/sysrq-trigger
      ```
  - Use a **GDB client** to connect to the target via serial or network for remote debugging.

---

- **TRACING:**
  - There are two main types of tracing: **static** and **dynamic**.  
  - **Frameworks and tools using tracing features:**
    - **Ftrace**
    - **trace-cmd**
    - **KernelShark**
    - **SystemTap**
    - **perf**
    - **Kernel Live Patching**

---

