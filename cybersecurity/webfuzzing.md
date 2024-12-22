# Web Fuzzing
## What is fuzzing?

Fuzzing trying a lot of random or predefined inputs on an application.
It is different to Brute-Forcing in that you don't try to unlock a door with every key on the keyring,
you try to throw random things on the door that are not very systemtematic.

## Essential Concepts

**Wordlist**: A dictionary of words, phrases, filenames, directory names or values used as input during the fuzzing process
**Payload**: Actual data sent to the web application
**Response Analysis**: Seeing or examining the response sent by the web application
**Fuzzer**: A software that automates generating and sending payloads to a web app and examining the responses
**False Positive**: A result incorrectly determined by the fuzzzer as a vulnerability
**False Negative**: A result incorrectly determined by the fuzzer as no vulnerability
**Fuzzing Scope**: The parts of the applicaion that you are specifically targetting.

## Directory and file fuzzing

Web applications have often directories and files not linked or visible to the user. Fuzzing aims to discover these directories because they may include some sensitive information.

Commonly used wordlists:

- /usr/share/seclists/Discovery/Web-Content/common.txt: General porpose Wordlist

- /usr/share/seclists/Discovery/Web-Content/directory-list-2.3-medium.txt: Directory Focused Wordlist

- /usr/share/seclists/Discovery/Web-Content/raft-large-directories.txt: Large Directory Focused Wordlist

- /usr/share/seclists/Discovery/Web-Content/big.txt: Very big wordlist of both directories and files


## How recursive fuzzing works

1. Initial Fuzzing: The fuzzer starts at the top level directory `/` and starts the fuzzing process. It finds some directories.
2. Recursive branching: The fuzzer now takes these subdirectories and starts a fresh fuzzing process from this directory.
3. Iterative depth: The fuzzer does this again and again until a specific level gets rechead that can be specified.

Recursive fuzzing is about working **smarter, not harder**.


### ffuf (Fuzz Faster U Foool)

How does ffuf work.

1. **Wordlist**: You provide ffuf with a wordlist containing the potential names of the directories and files
2. **URL with FUZZ**: The url that you provide has the FUZZ keyword that gets replaced by the wordlist entries
3. **Requests**: ffuf iterates through the wordlist and sends the requests to the urls that are generated.
4. **Analyzing Responses**: ffuf now tries to analyze the servers response for any abnormailities

#### Directory Fuzzing

```shell
ffuf -w /usr/share/seclists/Discovery/Web-Content/directory-list-2.3-medium.txt -u http://IP:PORT/FUZZ
```

`-w` (wordlist) Path to the wordlist
`-u` (URL) Specifies the URL with the FUZZ keyword that gets fuzzed by ffuf

#### File Fuzzing

```shell
ffuf -w /usr/share/seclists/Discovery/Web-Content/common.txt -u http://IP:PORT/w2ksvrus/FUZZ.html -e .php,.html,.txt,.bak,.js -v 
```

#### Recursive Fuzzing in ffuf

Recursive fuzzing can be very resource intense. It is crucial to limit how much recursion you want and how many requests ffuf sends.

`-recursion-depth`: controls how many directories ffuf can get deeper
`-rate`: The maximal rate of requests per second, preventing that the server gets overloaded.
`-timeout`:  Sets the timout for invdividual requests

```
ffuf -w /usr/share/seclists/Discovery/Web-Content/directory-list-2.3-medium.txt -ic -u http://IP:PORT/FUZZ -e .html -recursion -recursion-depth 2 -rate 500
```

#### Examples
```shell
ffuf -fc 403 # filter out code 403
ffuf -ic # ignore Comments in wordlist
```
