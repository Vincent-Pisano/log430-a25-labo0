# LOG-430 - Labo 0

## Question 1 - Si l’un des tests échoue à cause d’un bug, comment pytest signale-t-il l’erreur et aide-t-il à la localiser ? 
- Rédigez un test qui provoque volontairement une erreur, puis montrez la sortie du terminal obtenue.

```
==================================================== test session starts =====================================================
platform win32 -- Python 3.10.6, pytest-8.4.2, pluggy-1.6.0
rootdir: C:\Users\vpisa\Documents\ETS\Code\LOG430\labos\log430-a25-labo0-fork\src
collected 3 items

tests\test_calculator.py ..F                                                                                            [100%]

========================================================== FAILURES ==========================================================
____________________________________________________ test_addition_bugged ____________________________________________________

    def test_addition_bugged():
        my_calculator = Calculator()
>       assert my_calculator.addition(2, 3) == 6
E       assert 5 == 6
E        +  where 5 = addition(2, 3)
E        +    where addition = <src.calculator.Calculator object at 0x00000141B3A63C10>.addition

tests\test_calculator.py:20: AssertionError
================================================== short test summary info ===================================================
FAILED tests/test_calculator.py::test_addition_bugged - assert 5 == 6
================================================ 1 failed, 2 passed in 0.11s =================================================
```
- Il écrit un "F" à côté du nom du fichier de test
- Il spécifie le nom du test qui a failed en rouge et montre les lignes en erreur en rouge également


## Question 2 - Que fait GitLab pendant les étapes de « setup » et « checkout » ? 
- Veuillez inclure la sortie du terminal Gitlab CI dans votre réponse.

- setup :
  - Il configure l'OS du runner qui va exécuter le workflow, sélectionne l'image à utiliser en fonction des paramètre mis dans le fichier de CI, confirme ses permissions GITHUB et télécharge les actions nécessaires pour exécuter le workflow
  - ```
    Current runner version: '2.328.0'
    Runner Image Provisioner
        Hosted Compute Agent
        Version: 20250908.385
        Commit: cfce01a78eb650f23de9981e2e79cb8dc83e6056
        Build Date: 2025-09-08T16:07:37Z
    Operating System
        Ubuntu
        24.04.3
        LTS
    Runner Image
        Image: ubuntu-24.04
        Version: 20250907.24.1
        Included Software: https://github.com/actions/runner-images/blob/ubuntu24/20250907.24/images/ubuntu/Ubuntu2404-Readme.md
        Image Release: https://github.com/actions/runner-images/releases/tag/ubuntu24%2F20250907.24
    GITHUB_TOKEN Permissions
        Actions: write
        Attestations: write
        Checks: write
        Contents: write
        Deployments: write
        Discussions: write
        Issues: write
        Metadata: read
        Models: read
        Packages: write
        Pages: write
        PullRequests: write
        RepositoryProjects: write
        SecurityEvents: write
        Statuses: write
    Secret source: Actions
    Prepare workflow directory
    Prepare all required actions
    Getting action download info
    Download action repository 'actions/checkout@v3' (SHA:f43a0e5ff2bd294095638e18286ca9a3d1956744)
    Download action repository 'actions/setup-python@v4' (SHA:7f4fc3e22c37d6ff65e88745f38bd3157c663f7c)
    Complete job name: build
    ```
- checkout :
  - Il exécute l'action native de github actions/checkout@v3 et fait un git clone du dépôt sur la bonne branche pour pouvoir l'exécuter
  - ```
    Run actions/checkout@v3
    Syncing repository: Vincent-Pisano/log430-a25-labo0
    Getting Git version info
    Temporarily overriding HOME='/home/runner/work/_temp/10d0bead-1575-44c5-b131-ea2925427eab' before making global git config changes
    Adding repository directory to the temporary git global config as a safe directory
    /usr/bin/git config --global --add safe.directory /home/runner/work/log430-a25-labo0/log430-a25-labo0
    Deleting the contents of '/home/runner/work/log430-a25-labo0/log430-a25-labo0'
    Initializing the repository
    Disabling automatic garbage collection
    Setting up auth
    Fetching the repository
    Determining the checkout info
    Checking out the ref
    /usr/bin/git log -1 --format='%H'
    '59810cb5c54b35f0d0082caed8239eb04b9ec08f'
    ```

## Question 3 - Quelle approche et quelles commandes avez-vous exécutées pour automatiser le déploiement continu de l'application dans la machine virtuelle ?
- Veuillez inclure les sorties du terminal et les scripts bash dans votre réponse.

- J'ai utiliser le script ci-dessous pour me connecter à la machine virtuelle :
-   ```
    sshpass -p "${{ secrets.SSH_PASSWORD }}" ssh -o StrictHostKeyChecking=no ${{ secrets.SSH_USER }}@${{ secrets.SSH_HOST }} << 'EOF'
        rm -rf log430-a25-labo0 || true
        git clone https://github.com/Vincent-Pisano/log430-a25-labo0.git
        cd log430-a25-labo0

        docker build -t labo0-calculator .

        docker stop labo0-calculator-container || true
        docker rm labo0-calculator-container || true

        docker run -d --name labo0-calculator-container labo0-calculator
    EOF
    ```
> [!WARNING] 
> Il ne fonctionne pas sur github car le host est une adresse privée !

- Ce script me permet de me connecter à la machine virtuelle en rentrant le mot de passe et user@host, puis j'ai refait un clone du repertoire Gitub, et j'ai relancer la commande de build

```
sshpass -p "MON_MOT_DE_PASSE" ssh -o StrictHostKeyChecking=no log430@10.194.32.250 << 'EOF'
        rm -rf log430-a25-labo0 || true
        git clone https://github.com/Vincent-Pisano/log430-a25-labo0.git
        cd log430-a25-labo0

        docker build -t labo0-calculator .

        docker stop labo0-calculator-container || true
        docker rm labo0-calculator-container || true

        docker run -d --name labo0-calculator-container labo0-calculator
EOF
Pseudo-terminal will not be allocated because stdin is not a terminal.
Welcome to Ubuntu 24.04.3 LTS (GNU/Linux 6.8.0-71-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/pro

 System information as of Wed Sep 10 13:43:04 UTC 2025

  System load:  0.0                Processes:               132
  Usage of /:   15.9% of 23.17GB   Users logged in:         1
  Memory usage: 14%                IPv4 address for enp5s0: 10.194.32.250
  Swap usage:   0%

 * Strictly confined Kubernetes makes edge and IoT secure. Learn how MicroK8s
   just raised the bar for easy, resilient and secure K8s cluster deployment.

   https://ubuntu.com/engage/secure-kubernetes-at-the-edge

Expanded Security Maintenance for Applications is not enabled.

5 updates can be applied immediately.
To see these additional updates run: apt list --upgradable

Enable ESM Apps to receive additional future security updates.
See https://ubuntu.com/esm or run: sudo pro status


*** System restart required ***
Cloning into 'log430-a25-labo0'...
#0 building with "default" instance using docker driver

#1 [internal] load build definition from Dockerfile
#1 transferring dockerfile: 128B done
#1 DONE 0.0s

#2 [internal] load metadata for docker.io/library/python:3.11-slim
#2 DONE 0.2s

#3 [internal] load .dockerignore
#3 transferring context: 2B done
#3 DONE 0.0s

#4 [1/3] FROM docker.io/library/python:3.11-slim@sha256:91e9d01cf4bd56be7128c603506b6fe367ef7506f9f2ad8f3a908aeec8941bb9
#4 DONE 0.0s

#5 [internal] load build context
#5 transferring context: 2.33kB done
#5 DONE 0.0s

#6 [2/3] WORKDIR /app
#6 CACHED

#7 [3/3] COPY src/ ./src/
#7 CACHED

#8 exporting to image
#8 exporting layers done
#8 writing image sha256:b4f50f9d5374e9a87d59838929fbf9bd2d1d944b3abd9cdff802423a5a8135d3 done
#8 naming to docker.io/library/labo0-calculator done
#8 DONE 0.0s
labo0-calculator-container
labo0-calculator-container
05278e5cdf9e5f6d9cd24470bc105eb05ab55c20615ebc48860929459bc789e4
```

## Question 4 - Quel type d'informations pouvez-vous obtenir via la commande « top » ? 
- Veuillez inclure la sortie du terminal dans votre réponse.
- La commande top permet de visualiser l'utilisation du CPU en temps réel ainsi que la liste des processus et leur informations

```
log430@log430-etudiante-5:~$ free -h
               total        used        free      shared  buff/cache   available
Mem:           3.5Gi       680Mi       1.4Gi        17Mi       1.8Gi       2.9Gi
Swap:             0B          0B          0B
log430@log430-etudiante-5:~$ top
top - 13:54:12 up 6 days, 16:06,  2 users,  load average: 0.00, 0.00, 0.00
Tasks: 135 total,   1 running, 134 sleeping,   0 stopped,   0 zombie
%Cpu(s):  4.3 us,  4.3 sy,  0.0 ni, 91.3 id,  0.0 wa,  0.0 hi,  0.0 si,  0.0 st
MiB Mem :   3622.6 total,   1412.9 free,    680.3 used,   1831.8 buff/cache
MiB Swap:      0.0 total,      0.0 free,      0.0 used.   2942.2 avail Mem

    PID USER      PR  NI    VIRT    RES    SHR S  %CPU  %MEM     TIME+ COMMAND
      1 root      20   0   22624  13944   9592 S   0.0   0.4   0:25.14 systemd
      2 root      20   0       0      0      0 S   0.0   0.0   0:00.18 kthreadd
      3 root      20   0       0      0      0 S   0.0   0.0   0:00.00 pool_workqueue_release
      4 root       0 -20       0      0      0 I   0.0   0.0   0:00.00 kworker/R-rcu_g
      5 root       0 -20       0      0      0 I   0.0   0.0   0:00.00 kworker/R-rcu_p
      6 root       0 -20       0      0      0 I   0.0   0.0   0:00.00 kworker/R-slub_
      7 root       0 -20       0      0      0 I   0.0   0.0   0:00.00 kworker/R-netns
     10 root       0 -20       0      0      0 I   0.0   0.0   0:00.00 kworker/0:0H-events_highpri
     12 root       0 -20       0      0      0 I   0.0   0.0   0:00.00 kworker/R-mm_pe
     13 root      20   0       0      0      0 I   0.0   0.0   0:00.00 rcu_tasks_kthread
     14 root      20   0       0      0      0 I   0.0   0.0   0:00.00 rcu_tasks_rude_kthread
     15 root      20   0       0      0      0 I   0.0   0.0   0:00.00 rcu_tasks_trace_kthread
     16 root      20   0       0      0      0 S   0.0   0.0   0:02.46 ksoftirqd/0
     17 root      20   0       0      0      0 I   0.0   0.0   0:58.13 rcu_preempt
     18 root      rt   0       0      0      0 S   0.0   0.0   0:04.96 migration/0
     19 root     -51   0       0      0      0 S   0.0   0.0   0:00.00 idle_inject/0
     20 root      20   0       0      0      0 S   0.0   0.0   0:00.00 cpuhp/0
     21 root      20   0       0      0      0 S   0.0   0.0   0:00.00 cpuhp/1
     22 root     -51   0       0      0      0 S   0.0   0.0   0:00.00 idle_inject/1
     23 root      rt   0       0      0      0 S   0.0   0.0   0:04.33 migration/1
log430@log430-etudiante-5:~$ df -h
Filesystem      Size  Used Avail Use% Mounted on
tmpfs           363M  1.3M  362M   1% /run
efivarfs        256K   32K  220K  13% /sys/firmware/efi/efivars
/dev/sda1        24G  3.7G   20G  16% /
tmpfs           1.8G     0  1.8G   0% /dev/shm
tmpfs           5.0M     0  5.0M   0% /run/lock
tmpfs            50M   13M   38M  26% /run/lxd_agent
/dev/sda16      881M  112M  708M  14% /boot
/dev/sda15      105M  6.2M   99M   6% /boot/efi
tmpfs           363M   12K  363M   1% /run/user/1000
```