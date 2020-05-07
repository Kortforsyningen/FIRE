.. _for_udviklere:

Udviklingsguide
=======================

Denne side indeholder alverdens information der er relevant for dem der arbejder
på kildekoden bag FIRE.

Opsætning
----------

.. note::

    Alt herunder forudsætter at det foretages i en Conda terminal. Se
    :ref:`installationsvejledningen <installation>` for mere om Conda.

Start med at clone git repositoriet::

    > git clone https://github.com/Kortforsyningen/FIRE.git

Et godt udviklingsmiljø at tage udgangspunkt i er `fire-dev.yaml`, som nemt
installeres med Conda. Fra fire git-repositoriet køres::

    > conda env create --file environment-dev.yml
    > conda activate fire-dev

Herefter burde alle de essentielle programmer og biblioteker være til rådighed.

Installer en udviklingsversion af fire med::

    > pip install -e .

Kodestil
--------

`fire` kommandoen og QGIS pluginen Flame taler dansk til brugeren. Dokumentation
skrives ligeledes på dansk. Det er tilladt at skrive kommentarer, funktions- og
variabelnavne på engelsk. git commits bør så vidt muligt skrives på dansk.

Al kode i fire repositoriet skal køres gennem ``black`` inden det committes.
Dette gøres for at skabe et ensartet udtryk på tværs af hele kodebasen. Desuden
har black den bivirking at forstyrrende, overflødige ændringer typisk forsvinder
fra diffs mellem to commits, hvilket gør det væsentligt nemmere at lave review
af kodeændringer.

Kør ``black`` med::

    > black .

Hvis ikke ``black`` er kørt inden kode pushes til GitHub vil CI tests fejle.


Dokumentation
-------------

HTML dokumentation kan genereres lokalt ved hjælp af følgende kommando::

    sphinx-build -b html ./docs ./docs/_build

hvorefter dokumentationen vil være at finde i ``docs/_build``.

Når der tilføjes eller fjernes moduler til API koden skal dokumentationen
opdaters (filer i ``docs/api``). Dette kan gøres med::

    cd docs
    sphinx-apidoc -e -o api ../fireapi


GitHub og Continuous Integration
---------------------------------

Fire repositoriet håndteres på GitHub, hvor der er sat en række Continous
Integration (CI) services op. Disse benyttes blandt andet til at afvikle test
suiten og til at generere HTML dokumentation efter hvert commit.

GitHub er konfigureret sådan at man ikke kan lave ``git push`` direkte til ``master``.
For at inkludere kode i ``master`` kræves det at man laver et pull request med mindst
et godkendt review fra en kollega, samt at alle CI tests gennemføres successfuldt.

QGIS Plugin
------------

Flame pakkes til release ved brug af ``pb_tool``::

    > cd flame
    > pb_tool zip

hvorefter filen ``flame_plugin.zip`` placeres i ``flame/zip``.

Mere om ``pb_tool`` her https://github.com/g-sherman/plugin_build_tool.