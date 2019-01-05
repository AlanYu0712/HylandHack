import os
import shutil
import time
from watchdog.events import FileSystemEventHandler
from watchdog.observers import Observer
import os
import sys

print("Hello, Welcome to Sparrow's Compass")
print("Let's get you started")
print("Scanning your Download Target Folder...")

def get_download_path():
    """Returns the default downloads path for linux or windows"""
    if os.name == 'nt':
        import winreg
        sub_key = r'SOFTWARE\Microsoft\Windows\CurrentVersion\Explorer\Shell Folders'
        downloads_guid = '{374DE290-123F-4565-9164-39C4925E467B}'
        with winreg.OpenKey(winreg.HKEY_CURRENT_USER, sub_key) as key:
            location = winreg.QueryValueEx(key, downloads_guid)[0]
        return location
    else:
        return os.path.join(os.path.expanduser('~'), 'downloads')


result = get_download_path()+"\\"

print("\nwe found you usually download your file to the " + result + " Folder.")


while True:
    DownloadFold = input("Press 'C' to confirm \nOR \nPress 'R' to relocate folder\n to continue setup\n")
    if DownloadFold.lower() == 'c':
        print("Great!\n")
        break
    elif DownloadFold.lower() == 'r':
        while True:
            Nresult = input("type a valid folder path\n")
            if os.path.isdir(Nresult):
                if Nresult[-1] == "\\":
                    result = Nresult
                else:
                    result = Nresult+"\\"
                print("alright, you new target folder is " + result + "!")
                break
            else:
                print("invalid directory, lad!")
                continue
        break
    else:
        print("I don't understand that, please try again")

print("\nLet's now setup how would you filter your files")

FilNames = list()
FilDirs = list()
key = 0

while key == 0:
    FilName = input("specific key words of the file name to filter the file (enter [skip] to skip this step)\n")
    if FilName == "[skip]":
        break
    #todo: Check input
    while True:
        cAsk = input("Should this key word be case Sensitive?\nYes[Y]\tNo[N]\n")
        if cAsk.lower() == 'y':
            FilNames.append(FilName)
            break
        elif cAsk.lower() == 'n':
            FilName = FilName.lower()
            FilNames.append(FilName)
            break
        else:
            print("You've use the compass in a wrong way, do it the right way, Saavy")
            continue
    print(FilNames)
    print("\nAnd you would like to files with name that contains " + FilName + " to")

    #todo: Check input
    while True:
        op = input(" an existing folder [E]\nOR\ncreate a new folder[C]\n")
        if op.lower() == "e":
            while True:
                FilDir = input("input a valid directory then\n")
                if os.path.isdir(FilDir):
                    if FilDir[-1] == "\\":
                        FilDirs.append(FilDir)
                    else:
                        FilDirs.append(FilDir+"\\")

                    break
                else:
                    print("directory invalid")
                    continue
            break
        elif op.lower() == 'c':
            FoldName = result + input("The folder name will be:")
            os.makedirs(FoldName)
            print(FoldName)
            break
        else:
            print("Do it the right way, lad!")
    # todo: Check input
    while True:
        cont = input("Add another keyword filter?\nYes[Y]\tNo[N]\n")
        if cont.lower() == 'y':
            key = 0
            break
        elif cont.lower() == 'n':
            key = 1
            break
        else:
            continue


print("return code successfully")
