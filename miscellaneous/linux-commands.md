# Linux Commands

## Terminal

| **Command**                                      | **Description**                                                                                                                                                                                         |
| ------------------------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| find \</path> -regex ".\*.(jpg\|gif\|png\|jpeg)" | find file with extension                                                                                                                                                                                |
| diff -y <(xxd foo1.bin) <(xxd foo2.bin)          | xxd is hexdump.`-y` shows you differences side-by-side (optional).`xxd` is CLI tool to create a hexdump output of the binary file. Add `-W200` to `diff` for wider output (of 200 characters per line). |
| colordiff -y <(xxd foo1.bin) <(xxd foo2.bin)     | If you've `colordiff`, it can colorize `diff` output,                                                                                                                                                   |

