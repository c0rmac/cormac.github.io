[Féach an cód github](https://github.com/c0rmac/interlink-alg)

Chuir comhghleacaí liom an t-eagrán seo a leanas ó bhailiúchán cód DNA i láthair dom.

Bíodh carachtar aibítre ag cód DNA. Cuir i gcás go bhfuil foclóir ann ina bhfuil seicheamh cód DNA.
``` python
foclóir = ["A", "AB", "B", "BC", "C", "D", "CH", "E", "F", "EFG", "G", "FG" , "H"]
```
Go neamhfhoirmiúil, ba mhaith linn gach cód DNA a fhreagraíonn le cód eile a ghrúpáil le chéile sa seicheamh céanna CHOMH MAITH le haon
sheicheamh eile san bhfoclóir. Cruthóidh sé seo sraith tacair ar a dtugtar an tacar aitheantóirí - sraith de na grúpaí éagsúla
a fhreagraíonn le cód a chéile.

Ba chóir go bhfaighfear:
``` python
foclóir = [{'B', 'H', 'A', 'C'}, {'D'}, {'G', 'E', 'F'}]
```

## Cur Síos Foirmiúil

Bíodh $\text{Alpha}$ ina aibítir agus bíodh $\text{Dict}$ ina fhoclóir.

Bíodh $\text{Seq}(y) \in \text{Dict}$ ina sheicheamh ar bith ina bhfuil cód $y$.

Sainmhínítear an gaol coibhéise trí $x \sim y \iff \exists \text{Seq}(y): x \in \text{Seq}(y)$

Ansin sainmhínítear an tacar aitheantóirí mar:

$$
\text{Identifiers} = \text{Alpha} / \sim
$$

Is é an cuspóir baill an tacair $\text{Identifiers}$ a aimsiú.

## Castacht Ama

Is é castacht iomlán ama an algartam ná:

$$
O(M \times L^ 2 + 2C + C^2)
$$

áit a bhfuil:
- $M = |\text{Dict}|$ is ea líon na bhfocal.
- $L$ mar mheánfhad na bhfocal.
- $C=|\text{Alpha}|$ is ea líon na gcarachtar uathúla san aibítir.

## Algartam Description
### 1. Cuir Struchtúir Sonraí i dTús:
- **CharMap (`char_to_chars`)**: Mapáil a nascann gach carachtar le sraith de charachtair eile a bhfuil sé comhtharlaithe leo i bhfocail.
- **Eilimintí gan réiteach (`unravelled_elements`)**: Sraith chun súil a choinneáil ar charachtair atá próiseáilte.
- **Aitheantóirí (`identifiers`)**: Liosta chun tacair de charachtair ghaolmhara a stóráil a shásaíonn an gaol coibhéise.

### 2. Tóg an léarscáil `char_to_chars`:

- **Ionchur**: An Foclóir.
- **Próiseas**:
  - Téigh trí gach focal san fhoclóir.
  - I gcás gach carachtar san bhfocal, mapáil é le gach carachtar eile san bhfocal céanna.
  - Cruthaíonn sé seo struchtúr cosúil le graif inar carachtair iad nóid agus cuireann na himill comhtharlú na bhfocal in iúl.
- **Castacht Ama**: $O(M \times L^2)$
  - áit inarb é $M$ líon na bhfocal agus gurb é $L$ meánfhad na bhfocal.

### 3. Slabhraí Trasnaithe agus Sainaithin Eilimintí:

- **Ionchur**: An léarscáil `char_to_chars`.
- **Próiseas**:
  - Túsaigh an sraith fholamh (`unravelled_elements`) le carachtair phróiseáilte a rianú.
  - Téigh trí gach carachtar sa léarscáil `char_to_chars`.
  - I gcás gach carachtar neamhphróiseáilte, déan trasbhealach le gach carachtar gaolmhar a shainaithint.
  - Déan iniúchadh go hathchúrsach ar charachtair nasctha agus cuir leis an ngrúpa reatha iad (`element_identifier`).
  - Cuir an grúpa sainaitheanta de charachtair gaolmhara leis an liosta deiridh aitheantóirí.
- **Castacht Ama**: $O(2C + C^2)$
  - áit inar é $C$ líon na gcarachtar uathúla san aibítir.

### 4. Seol na Grúpaí Aitheanta ar ais:

- Tugann an algartam liosta tacair ar ais ina bhfuil carachtair i ngach tacar a bhaineann leis an tacar sonraí focal.