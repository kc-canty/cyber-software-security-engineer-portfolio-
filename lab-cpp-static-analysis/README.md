# Lab: C++ Static Security Analysis

## Overview
This lab demonstrates my ability to identify insecure C++ coding patterns using static analysis tools.  
It directly aligns with responsibilities of a Cyber Software Security Engineer, where detecting and preventing vulnerabilities early in the development lifecycle is critical.

The goal is to simulate how a DevSecOps pipeline would catch unsafe coding practices before software is deployed.

---

## Objectives
- Analyze a C++ source file containing intentional security flaws.
- Run industry-standard static analysis tools (cppcheck, flawfinder).
- Interpret vulnerability warnings and map them to CWE identifiers.
- Document remediation strategies and security impact.
- Explain why static analysis is essential in secure software engineering.

---

## Tools Used
- **cppcheck** – static analysis tool that detects coding errors & unsafe constructs  
- **flawfinder** – finds common C/C++ vulnerabilities based on secure coding rules  
- **VS Code** – code inspection and manual review  
- **Git** – version control & documentation workflow  

---

## Source File Analyzed
The following intentionally vulnerable C++ file was used:

```
#include <iostream>
#include <cstring>
using namespace std;

void insecure_copy(char *input) {
    char buffer[10];
    strcpy(buffer, input); // Insecure: No bounds checking
    cout << "Copied: " << buffer << endl;
}

int main(int argc, char *argv[]) {
    char userInput[256];
    cout << "Enter input: ";
    cin >> userInput; // Insecure: No input length validation
    insecure_copy(userInput);
    return 0;
}
```

### Intentional Vulnerabilities Included:
- Unsafe string copy (`strcpy`)
- Fixed-size buffer with no bounds checking
- Unvalidated input size
- Unbounded `cin >>`
- Potential stack-based buffer overflow

These patterns are commonly seen in legacy C/C++ codebases used in embedded, aerospace, and defense systems.

---

## Commands Used

### 1. Run cppcheck
```bash
cppcheck vulnerable.cpp --enable=all --inconclusive --output-file=analysis-results/cppcheck-results.txt
```

### 2. Run flawfinder
```bash
flawfinder vulnerable.cpp > analysis-results/flawfinder-results.txt
```

### 3. Manual Review Notes
Documented in:
```
analysis-results/summary-findings.md
```

---

## Sample Output (Summarized)

### cppcheck flagged:
- `strcpy` as **unsafe function usage**
- Buffer `char buffer[10]` as **potential overflow**
- Missing input validation
- Unvalidated user input (`cin >>`)

### flawfinder flagged:
- `strcpy` → **CWE-120: Buffer Copy without Checking Size**
- Input handling leading to **CWE-20: Improper Input Validation**
- Potential **CWE-119: Buffer Overflow**

Both tools independently confirmed that the code is vulnerable to memory corruption and exploitation.

---

# Why These Steps Matter

### Why static analysis?
Static analysis finds vulnerabilities **before software runs**, making it:
- Faster
- Cheaper
- Easier to fix
- Mandatory in DoD & aerospace development pipelines

### Why detect buffer overflows?
Because buffer overflows can lead to:
- Arbitrary code execution  
- Privilege escalation  
- System crashes  
- Compromise of mission-critical systems  

This is especially important for aerospace, embedded systems, and military software.

### Why use multiple tools?
Different tools detect different patterns.  
Using cppcheck + flawfinder demonstrates:
- Toolchain diversity  
- Engineering maturity  
- Understanding secure coding workflows  

---

# Screenshot Checklist
Place in `/screenshots/`:
- `vscode_source_view.png` – showing vulnerable.cpp open
- `cppcheck_terminal.png` – showing cppcheck output
- `flawfinder_terminal.png` – showing flawfinder output

These make the lab visually professional for hiring managers.

---

# Key Findings (From summary-findings.md)

### Vulnerabilities Identified
| Issue | CWE | Severity | Description |
|-------|------|----------|-------------|
| Unsafe strcpy | CWE-120 | High | No bounds checking, potential overflow |
| Fixed buffer size | CWE-119 | High | Stack-based overflow risk |
| Unvalidated input | CWE-20 | Medium | No input length constraints |
| No sanitization | CWE-116 | Low | Output not sanitized |

---

# Proposed Remediation
- Replace `strcpy` with `strncpy` or `strlcpy`  
- Validate input length before copying  
- Use safer patterns such as `std::string`  
- Apply input sanitization  
- Implement bounds checking  
- Apply compiler hardening flags (`-fstack-protector`, ASLR, etc.)

---

# Final Summary (Interview-Safe Explanation)

### **What I Did**
I analyzed a purposely vulnerable C++ file using two static analysis tools (cppcheck & flawfinder), documented the results, and explained how to resolve each finding.

### **Why I Did It**
To simulate a real-world DevSecOps pipeline phase where insecure coding patterns are caught **before deployment**, exactly as Lockheed Martin expects for secure embedded and aerospace systems development.

### **What It Impacts**
Static analysis impacts:
- **Software reliability**  
- **Memory safety**  
- **Mission-critical system stability**  
- **Vulnerability exposure**  
- **Overall system hardening**  

### **What Impacts It**
The accuracy of static analysis depends on:
- Code quality  
- Coding standards (CERT, MISRA, JSF)  
- Developer awareness  
- Development lifecycle maturity  
- Tools used and rulesets enabled  

### **How I Will Explain This in an Interview**
> “I built a C++ static analysis lab where I intentionally introduced unsafe coding patterns, then used cppcheck and flawfinder to identify vulnerabilities like buffer overflows and unsafe string operations. I documented each finding, mapped them to CWE classifications, and wrote remediation steps.  
>  
> This lab shows I understand secure coding, static analysis workflows, DevSecOps principles, and how vulnerabilities impact embedded and mission-critical systems — all essential for a Cyber Software Security Engineer role.”

---

# End of Lab

