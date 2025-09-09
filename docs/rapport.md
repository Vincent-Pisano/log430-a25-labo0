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

## Question 3 - Quel approache et quelles commandes avez-vous exécutées pour automatiser le déploiement continu de l'application dans la machine virtuelle ?
- Veuillez inclure les sorties du terminal et les scripts bash dans votre réponse.

## Question 4 - Quel type d'informations pouvez-vous obtenir via la commande « top » ? 
- Veuillez inclure la sortie du terminal dans votre réponse.

