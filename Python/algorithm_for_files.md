# Algorithm for File Updates in Python

## Project description
At a healthcare organization, managing network access to sensitive patient records is critical for maintaining regulatory compliance and patient privacy. Access to these restricted subnetworks is controlled through an IP allow list stored in a text file. When employees transfer roles or leave the organization, their IP addresses are placed on a removal list and must be promptly expunged from the active allow list. In this project, I developed an automated Python algorithm that reads the allow list file, compares it against a removal list, safely removes the unauthorized IP addresses, and overwrites the file with the updated records.

---

## Open the file that contains the allow list
To start the algorithm, the target file name was assigned to the variable `import_file`. A `with` statement was used along with the `open()` function in read mode (`"r"`) to safely manage the file descriptor and ensure automatic closure upon completion.

```python
import_file = "allow_list.txt"

with open(import_file, "r") as file:
```

## Read the file contents
Inside the context manager block, the `.read()` method was used to convert the raw contents of `allow_list.txt` into a single string. This string was stored in a variable called `ip_addresses` for subsequent data manipulation.

```
with open(import_file, "r") as file:
    ip_addresses = file.read()
```

## Convert the string into a list
Because strings are immutable and cannot have individual items removed easily, the `ip_addresses` string needed to be converted into a structured list. The `.split()` method was applied, which by default splits the string on whitespace and newlines, converting each IP address into an individual list element.

`ip_addresses = ip_addresses.split()`

## Iterate through the remove list
A predefined list called `remove_list` contains the specific IP addresses that need to be revoked. A `for` loop was constructed to iterate through each entry in `remove_list`, using `element` as the loop variable.

```
remove_list = ["192.168.97.225", "192.168.158.170", "192.168.201.40", "192.168.58.57"]

for element in remove_list:
    # Evaluation and removal logic applied here
```
## Remove IP addresses that are on the remove list
Within the loop, a conditional statement evaluates whether the current element exists inside the `ip_addresses` list. If the condition is met, the `.remove()` method deletes that specific IP address from `ip_addresses`.
Applying the `.remove()` method in this way is possible because there are no duplicate entries in the `ip_addresses` list.

```
for element in remove_list:
    if element in ip_addresses:
        ip_addresses.remove(element)
```

## Update the file with the revised list of IP addresses
To finalize the update, the cleaned list of IP addresses must be written back to `allow_list.txt`. First, the `.join()` method was applied to the newline character `"\n"` to convert `ip_addresses` back into a newline-separated string. Then, another with statement was opened with the write flag (`"w"`), and the `.write()` method overwrote the existing file with the updated access list.

```
ip_addresses = "\n".join(ip_addresses)

with open(import_file, "w") as file:
    file.write(ip_addresses)
```
---

# Summary
This algorithm automates the maintenance of access control lists by updating authorized IP addresses in a local file. The script first opens and reads `allow_list.txt`, converting the raw string data into a manageable Python list using `.split()`. It then iterates through a target removal list, checking for matching entries and stripping out revoked IP addresses using conditional checks and `.remove()`. Finally, the algorithm reformats the revised list into a clean, newline-delimited string using `"\n".join()` and overwrites the original file using write mode. This automated workflow reduces human error and ensures that network access restrictions remain accurate and up to date.
