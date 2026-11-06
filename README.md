# Geonovum-software-template
Repository voor geonovum software projecten met geautomatiseerde generatie van SBOM, Publiccode.yaml en ontwikkeling van SBOS

## SBOM
Software bill of materials gestructureerde inventaris van alle softwarecomponenten https://developer.overheid.nl/kennisbank/security/standaarden/sbom

Deze template maakt bij elke release automatisch een SBOM, volgens de [2026 CISA Minimum Elements](https://www.cisa.gov/resources-tools/resources/2026-minimum-elements-software-bill-materials-sbom):

- [`sbom-inventory.json`](sbom-inventory.json) is de inventaris: het product en alle componenten van derden, zoals bibliotheken, meegeleverde bestanden, CDN-links en wat de server moet leveren.
- [`.github/workflows/sbom.yml`](.github/workflows/sbom.yml) maakt daaruit bij elke gepubliceerde GitHub-release een CycloneDX 1.7-SBOM met VEX en een SPDX 2.3-SBOM. De workflow valideert beide, ondertekent ze met een build-attestatie en hangt ze als asset aan de release.
- [`.github/workflows/sbom-kwetsbaarheden.yml`](.github/workflows/sbom-kwetsbaarheden.yml) legt elke maandag de SBOM van de release die in gebruik is naast [OSV](https://osv.dev), en opent een issue met het label `sbom` als er kwetsbaarheden te beoordelen zijn. Dat staat los van de build: de kwetsbaarheden van een versie veranderen ook als er geen nieuwe release komt.
- [`.github/dependabot.yml`](.github/dependabot.yml) houdt de actions in de workflows actueel. Die zijn vastgezet op een commit-hash.

De scripts komen uit de skill [`geonovum-sbom`](https://github.com/developer-overheid-nl/skills-sbom), release v0.0.2, vastgezet op de commit-hash daarvan (`SKILL_REF` in beide workflows). Voor een nieuwere release van de skill pas je `SKILL_REF` met de hand aan.

### Na het aanmaken van een repository met deze template

1. Vul `sbom-inventory.json` in. Vervang alle `VUL-IN`-waarden en zet de componenten erin. Het eenvoudigst gaat dat met de skill `geonovum-sbom`: vraag je assistent om een SBOM-inventaris voor de repository. Het formaat staat in [`inventaris-formaat.md`](https://github.com/developer-overheid-nl/skills-sbom/blob/v0.0.2/skills/geonovum-sbom/inventaris-formaat.md). Zolang er nog `VUL-IN` in staat, stopt de release-workflow met een foutmelding.
2. Liggen de versies pas vast na een build of installatie, bijvoorbeeld bij een lockfile (`package-lock.json`, `poetry.lock` en dergelijke) of bij versiebereiken, vul dan in `sbom.yml` de stap "Bouwen en afhankelijkheden lezen" in die als commentaar klaarstaat. Neem de uitvoer op via `import` in de inventaris.
3. Publiceer een release. De SBOMs verschijnen als assets bij die release. Zonder release kun je de workflow ook met de hand starten via het tabblad Actions; de SBOMs staan dan als artefact bij de run.
4. Draait er een andere release dan de laatste, zet dan de tag daarvan in de repository-variabele `SBOM_IN_GEBRUIK`. De wekelijkse controle kijkt dan naar die SBOM.
5. Werk de inventaris bij als er een component bijkomt, verdwijnt of een andere versie krijgt. Leg beoordelingen van kwetsbaarheden vast in het `vex`-blok; de wekelijkse controle neemt ze over.

In deze template-repository zelf draaien de workflows niet.

## publiccode.yaml
Metadata voor vindbaarheid software in opensource software register van de overheid https://developer.overheid.nl/kennisbank/open-source/standaarden/publiccode-yml

## SBOS
Software bill of standards, idee van project AI ready standaarden & compliance push om vergelijkbaar met SBOM expliciet aan te geven welke (versies van) standaarden een software component ondersteund 
