
# Simple styles for code development and execution

## 1. Overview of styles for code development and execution

-In the monolithic (full) style, the code development is fully and the code execution is fully. In this case, the code is written in a single file and executed as a single file.

-In the modular (partial) style, code development is partial and code execution is partial. In this case, the code is written in several files (modules) and executed in parts.

-In the combined (hybrid) style, code development is partial and code execution is full. In this case, the code is written in several files and executed as a single file in random access memory (RAM).

//In my previous github repositories, I mainly use the combined (hybrid) style.

Simplified "run" script for merging code using the combined style:
```
import os
import glob
from tqdm import tqdm

# List of files to execute
FILES_TO_EXECUTE = [
"1_imports.py",
"2_config.py",
"3_................ py",
"4_................... py",
"5_.................... py",
"6_...................... py",
"7_main.py"
]

def execute_py_files_in_memory(root_folder):
"""Combines all files into a single code block and executes it at once"""
all_py_files = {os.path.basename(f): f for f in glob.glob(os.path.join(root_folder, "*.py"))}

missing_files = []
combined_code = ""

# Read all files and collect them into a single string
for fname in FILES_TO_EXECUTE:
if fname in all_py_files:
with open(all_py_files[fname], "r", encoding="utf-8") as f:
code = f.read()
combined_code += f"\n# ----- {fname} -----\n"  # debug marker
combined_code += code + "\n"
else:
missing_files.append(fname)

if missing_files:
print("\n⚠️  The following files were not found:")
for f in missing_files:
print(f"   - {f}")

if not combined_code:
print("\n❌ No files available for execution.")
return

# Single global namespace for all executed code
global_namespace = {
"__name__": "__main__",
"__file__": os.path.join(root_folder, "__loader__.py"),
"root_folder": root_folder
}

print("\n" + "-" * 30)
    print(" ⚠️Executing all modules in RAM...")
print("-" * 30)

# Progress bar based on the total size of the combined code
total_size = len(combined_code.encode("utf-8"))
with tqdm(total=total_size, unit="B", unit_scale=True, desc="Running") as pbar:
try:
exec(combined_code, global_namespace)
pbar.update(total_size)
except Exception as e:
            print(f"\n ⚠️Execution failed: {e}")
return None

print("-" * 30)
return global_namespace

if __name__ == "__main__":
root_folder = os.path.dirname(os.path.abspath(__file__))
ns = execute_py_files_in_memory(root_folder)
```


## 2. Additional notes about the combined (hybrid) style of code development and execution

- Depending on the prevailing style of code development and execution, the combined style has two varieties: 1. combined style with monolithic (full) predominance; 2. combined style with modular (partial) predominance.

-With the combined style with a monolithic (full) predominance, the development is modular (partial), and the execution of the code and scripts is full. 

//With the regular combined style, you have one monolithic script, which is divided into modules (parts) and is executed as one script through the "run" script. (1 script in parts is executed as a whole)

//In the combined monolithic (full) predominance style, you have two or more ordinary monolithic scripts, which are divided into modules (parts) and run as a single script via the "run" script. That is just merging scripts. (2 or more monolithic scripts that are divided into parts are executed as a whole)

-With the combined style with modular (partial) predominance, the development is double modular (partial), and the execution of the code and scripts is full. 

//In the combined modular (partial) predominance style, you have two or more ordinary module scripts that are divided into submodules (subparts) and run as a single script by the "run" script. (2 or more modular scripts that are divided into parts run as a whole)

## 3. Applicability

-Combined style = modular (partial) style for code development + monolithic (full) style for code execution

-Combined style with monolithic (full) predominance = monolithic (full) style for script development + modular (partial) style for code development + monolithic (full) style for code execution

-Combined style with modular (partial) predominance = modular (partial) style for script development + modular (partial) style for code development + monolithic (full) style for execution

//Modules can be divided into parts, so the combined style with modular (partial) predominance can also be called double modular (partial).

-In the combined style, all parts are collected in the RAM and executed as a single script that is not available as a separate file on the disk.

-In the combined style, the script can be divided more freely than in the modular (partial) style. With the combined style, you can split an entire script line by line, and even each line can be in a separate script and still work (without hindering execution). This is an example of the versatility of the combined style.

-With the combined style, there are no imports in the individual modules (parts), because in reality you just assemble parts of one whole script.

//Through the combined style, the advantages of the monolithic (full) style and those of the modular (partial) style can be used. With the combination, you get the best of them.

Notes:

-Two or more monolithic (full) scripts can be executed one after the other.

-Two or more modular (partial) scripts can be executed one after the other.


-Two or more "run" scripts can be executed for a combined style, so that the scripts in the list can be executed linearly (one after the other) or in parallel.

-You can divide into parts the modules of scripts created using a modular (partial) style.

-A single namespace is used and there are no problems with imports and other code dependencies.

-The combined scripting style is good for error detection and function monitoring, because each function can be monitored separately by the "run" script, which is a mediator in the data transfer (but only if code is added for this).

-Through the combined style, you can manage the performance in parts. If you want, you can load only some modules (parts) of the script or execute them conditionally (if, else) by adding functions.
For some of the mentioned things, such as parallel execution and process monitoring, it is necessary to further develop the simplified "run" script for the specific purpose.

-The combined style can be used in programming languages other than Python, because the principle is simple and universal. 

//Maybe it's not suitable for teamwork.

