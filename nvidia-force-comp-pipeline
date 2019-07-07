#!/usr/bin/env python3
import sys
import subprocess

def main():

    metamode = _get_current_metamode()
    print("\nCurrent metamode is:\n%s" % metamode)
    
    if metamode == "":
        sys.exit(1)

    new_metamode = _add_force_comp_pipeline(metamode)
    print("\nNew metamode is:\n%s" % new_metamode)

    _apply_metamode(new_metamode)
    print("\nApplied new metamode.")

def _get_current_metamode():

    ret = subprocess.run(["nvidia-settings", "--query", "CurrentMetaMode"], stdout=subprocess.PIPE, stderr=subprocess.PIPE)

    stdout = ret.stdout.decode('utf-8')[:-1]
    stderr = ret.stderr.decode('utf-8')[:-1]

    if stderr != "": print(stderr)

    items_list = stdout.split("::")

    if len(items_list) != 2:
        print("Error parsing metamodes : more than one \"::\"-delimited fields found")
        return ""

    metamode = items_list[1][1:].replace("\n", "")

    return metamode

def _add_force_comp_pipeline(metamode):
    return metamode.replace("{", "{ForceCompositionPipeline=On, ")

def _apply_metamode(metamode):

    ret = subprocess.run(["nvidia-settings", "--assign", "CurrentMetaMode=%s" % metamode], stdout=subprocess.PIPE, stderr=subprocess.PIPE)

    stderr = ret.stderr.decode('utf-8')[:-1]
    if stderr != "": print(stderr)


if __name__ == "__main__":
    main()
