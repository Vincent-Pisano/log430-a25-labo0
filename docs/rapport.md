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
  - Il configure l'OS du runner qui va exécuter le workflow, sélectionne l'image à utiliser en fonction des paramètre mis dans le fichier de CI, confirme ses permissions GITHUB et télécharge les actions nécessairent pour exécuter le workflow
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

## Question 3 - Quel approache et quelles commandes avez-vous exécutées pour automatiser le déploiement continu de l'application dans la machine virtuelle ?
- Veuillez inclure les sorties du terminal et les scripts bash dans votre réponse.

## Question 4 - Quel type d'informations pouvez-vous obtenir via la commande « top » ? 
- Veuillez inclure la sortie du terminal dans votre réponse.

