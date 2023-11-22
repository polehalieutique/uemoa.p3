---
title: "UEMOA-RAPPORT"
author: "Jerome Guitton, Faustin Hounkpatin, Mohamed Soumah"
date: '2023-07-13'
always_allow_html: true
output:
  html_document: 
   toc: yes
   toc_float: true
   theme: united
   number_sections: true
  pdf_document: default
---






```r
unique(toupper(donnees$effectif_site_debar$pays))->pays

paste("Rapport du ",date(),"pour ",pays,sep=' ')
```

```
## [1] "Rapport du  Wed Nov 22 23:02:33 2023 pour  SEN"
```
# 1.	Contexte et objectifs

Mettre ici les contextes et objectifs

# 2.	Méthodologie

Moyens réunis et calendrier
-	Nombre d’enquêteurs mobilisés




## Base de sondage des sites de débarquement/vente en frais, stratifié par niveau adm. 1

## Types d’enquêtes déployées

	
-	Période de l’enquête terrain
-	Exhaustivité/échantillonnage
-	Nbre de sites enquêtés, nbre d’opérateurs interviewés

### Suivi des effectifs 

```r
donnees$effectif_site_debar %>% 
  group_by(niveau_adm1) %>% 
  summarise(nb.enquete=n())->sites.enquete

sites.enquete %>% kbl() %>%  kable_styling()
```

<table class="table" style="margin-left: auto; margin-right: auto;">
 <thead>
  <tr>
   <th style="text-align:left;"> niveau_adm1 </th>
   <th style="text-align:right;"> nb.enquete </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:left;"> Dakar </td>
   <td style="text-align:right;"> 19 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> fatick </td>
   <td style="text-align:right;"> 1 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Fatick </td>
   <td style="text-align:right;"> 42 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Kaolack </td>
   <td style="text-align:right;"> 10 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Louga </td>
   <td style="text-align:right;"> 3 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Saint Louis </td>
   <td style="text-align:right;"> 13 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Thies </td>
   <td style="text-align:right;"> 14 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> ziguinchor </td>
   <td style="text-align:right;"> 1 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Ziguinchor </td>
   <td style="text-align:right;"> 16 </td>
  </tr>
</tbody>
</table>

```r
ggplot(sites.enquete)+geom_bar(aes(x=niveau_adm1,y=nb.enquete),stat='identity')+
  coord_flip()
```

<img src="/tmp/RtmpItTkgh/preview-2658e22e89d0f4.dir/Rapports_base2_files/figure-html/unnamed-chunk-2-1.png" width="672" />



```r
rm(liste.sites)
```

```
## Warning in rm(liste.sites): objet 'liste.sites' introuvable
```

```r
liste.sites.init<-donnees$effectif_site_debar %>% mutate(longitude=as.numeric(Long_dec),latitude=as.numeric(lat_dec))


liste.sites<-st_as_sf(filter(liste.sites.init,!is.na(longitude) & !is.na(latitude)), coords = c("longitude", "latitude"),remove=FALSE, crs = 4326, agr = "constant")

leaflet(liste.sites %>% filter(latitude!=0 ) %>% st_drop_geometry() %>% 
  dplyr::mutate(latitude=as.numeric(latitude),longitude=as.numeric(longitude))) %>% addProviderTiles("Esri.WorldImagery")  %>%
  addCircleMarkers(radius = 10,
    color = 'red',
    stroke = FALSE, fillOpacity = 0.5) %>% 
   addLabelOnlyMarkers(~longitude,~latitude, label =  ~as.character(nom_site), 
                      labelOptions = labelOptions(noHide = T, direction = 'top', textOnly = T))
```

```
## Assuming "longitude" and "latitude" are longitude and latitude, respectively
```

```{=html}
<div class="leaflet html-widget html-fill-item-overflow-hidden html-fill-item" id="htmlwidget-da8c34e3b17979d554ef" style="width:672px;height:480px;"></div>
<script type="application/json" data-for="htmlwidget-da8c34e3b17979d554ef">{"x":{"options":{"crs":{"crsClass":"L.CRS.EPSG3857","code":null,"proj4def":null,"projectedBounds":null,"options":{}}},"calls":[{"method":"addProviderTiles","args":["Esri.WorldImagery",null,null,{"errorTileUrl":"","noWrap":false,"detectRetina":false}]},{"method":"addCircleMarkers","args":[[14.1836109161377,14.2811107635498,14.1180553436279,14.852222442627,14.1513891220093,14.1999998092651,14.2163000106812,14.3077774047852,14.4658336639404,14.6772222518921,14.6605558395386,13.7166662216187,13.7166662216187,13.6833333969116,14.1252775192261,14.140832901001,14.8999996185303,14.635555267334,14.4905557632446,14.5825004577637,15.1855554580688,14.9913892745972,14,14.1099996566772,14.1016664505005,14.1122226715088,14.1876945495605,14.1955556869507,14.154444694519,12.9949998855591,14.4658336639404,14.4658336639404,14.4658336639404,16.5533332824707,15.6258335113525,15.4488887786865,14.6688890457153,14.527777671814,14.5574998855591,14.5574998855591,15.8847217559814,15.8847217559814,15.8900003433228,15.8900003433228,14.8166666030884,13.1499996185303,13.1000003814697,13.1000003814697,13.1000003814697,13.8308000564575,13.75475025177,13.7541999816895,13.819450378418,13.7926998138428,13.7847003936768,15.8427782058716,15.8427782058716,15.5,15.5,14.7122220993042,14,14,14.7224998474121,14.1333332061768,14.7166662216187,14.0500001907349,14.0500001907349,14.2333335876465,14.1333332061768,14.0166664123535,15.9650001525879,13.9241666793823,13.9166669845581,14.0725002288818,13.8586111068726,13.8852777481079,13.9258337020874,13.9352874755859,12.3597221374512,12.4716663360596,16.0436420440674,16.0436611175537,14.6960000991821,14.6949996948242,14.6840000152588,14.6730003356934,14.6750001907349,14.628399848938,14.6135997772217,12.3030996322632,16.0436420440674,16.0105590820312,16.0160007476807],[-16.1622219085693,-16.0511112213135,-16.0858325958252,-16.111665725708,-16.2297229766846,-16.283332824707,-16.2216606140137,-16.1350002288818,-17.0611114501953,-17.4580554962158,-17.4325008392334,-16.5666675567627,-16.5666675567627,-16.6333332061768,-16.4672222137451,-16.47194480896,-17.1166667938232,-16.1516666412354,-16.0063896179199,-16.0030555725098,-16.9080562591553,-17.2855548858643,-17.3102779388428,-17.2883338928223,-17.2519435882568,-17.2544441223145,-16.3119449615479,-16.4174995422363,-16.4869441986084,-16.7383308410645,-17.0611114501953,-17.0611114501953,-17.0611114501953,-15.7419443130493,-16.6149997711182,-16.7377777099609,-16.1525001525879,-17.09694480896,-17.1138896942139,-17.1138896942139,-16.5005550384521,-16.5005550384521,-16.5100002288818,-16.5100002288818,-14.75,-16.1000003814697,-16.8333339691162,-16.8333339691162,-16.8999996185303,-16.1415996551514,-16.4789600372314,-16.5128002166748,-16.4538497924805,-16.4787006378174,-16.4783000946045,-16.5149993896484,-16.5149993896484,-16.4166660308838,-16.4166660308838,-17.280834197998,-17,-17,-17.3866672515869,-16.6499996185303,-16.6833324432373,-16.3333339691162,-16.5666675567627,-16.5166664123535,-16.6333332061768,-16.6666660308838,-13.6166667938232,-16.59055519104,-16.6166667938232,-16.1172218322754,-16.719165802002,-16.7291660308838,-16.6686115264893,-16.7614154815674,-16.6702785491943,-16.7855548858643,-16.508659362793,-16.508659362793,-17.2229995727539,-17.2409992218018,-17.2250003814697,-17.2129993438721,-17.2299995422363,-17.1658000946045,-17.1576995849609,-16.3955993652344,-16.508659362793,-16.3034000396729,-16.3034000396729],10,null,null,{"interactive":true,"className":"","stroke":false,"color":"red","weight":5,"opacity":0.5,"fill":true,"fillColor":"red","fillOpacity":0.5},null,null,null,null,null,{"interactive":false,"permanent":false,"direction":"auto","opacity":1,"offset":[0,0],"textsize":"10px","textOnly":false,"className":"","sticky":true},null]},{"method":"addMarkers","args":[[14.1836109161377,14.2811107635498,14.1180553436279,14.852222442627,14.1513891220093,14.1999998092651,14.2163000106812,14.3077774047852,14.4658336639404,14.6772222518921,14.6605558395386,13.7166662216187,13.7166662216187,13.6833333969116,14.1252775192261,14.140832901001,14.8999996185303,14.635555267334,14.4905557632446,14.5825004577637,15.1855554580688,14.9913892745972,14,14.1099996566772,14.1016664505005,14.1122226715088,14.1876945495605,14.1955556869507,14.154444694519,12.9949998855591,14.4658336639404,14.4658336639404,14.4658336639404,16.5533332824707,15.6258335113525,15.4488887786865,14.6688890457153,14.527777671814,14.5574998855591,14.5574998855591,15.8847217559814,15.8847217559814,15.8900003433228,15.8900003433228,14.8166666030884,13.1499996185303,13.1000003814697,13.1000003814697,13.1000003814697,13.8308000564575,13.75475025177,13.7541999816895,13.819450378418,13.7926998138428,13.7847003936768,15.8427782058716,15.8427782058716,15.5,15.5,14.7122220993042,14,14,14.7224998474121,14.1333332061768,14.7166662216187,14.0500001907349,14.0500001907349,14.2333335876465,14.1333332061768,14.0166664123535,15.9650001525879,13.9241666793823,13.9166669845581,14.0725002288818,13.8586111068726,13.8852777481079,13.9258337020874,13.9352874755859,12.3597221374512,12.4716663360596,16.0436420440674,16.0436611175537,14.6960000991821,14.6949996948242,14.6840000152588,14.6730003356934,14.6750001907349,14.628399848938,14.6135997772217,12.3030996322632,16.0436420440674,16.0105590820312,16.0160007476807],[-16.1622219085693,-16.0511112213135,-16.0858325958252,-16.111665725708,-16.2297229766846,-16.283332824707,-16.2216606140137,-16.1350002288818,-17.0611114501953,-17.4580554962158,-17.4325008392334,-16.5666675567627,-16.5666675567627,-16.6333332061768,-16.4672222137451,-16.47194480896,-17.1166667938232,-16.1516666412354,-16.0063896179199,-16.0030555725098,-16.9080562591553,-17.2855548858643,-17.3102779388428,-17.2883338928223,-17.2519435882568,-17.2544441223145,-16.3119449615479,-16.4174995422363,-16.4869441986084,-16.7383308410645,-17.0611114501953,-17.0611114501953,-17.0611114501953,-15.7419443130493,-16.6149997711182,-16.7377777099609,-16.1525001525879,-17.09694480896,-17.1138896942139,-17.1138896942139,-16.5005550384521,-16.5005550384521,-16.5100002288818,-16.5100002288818,-14.75,-16.1000003814697,-16.8333339691162,-16.8333339691162,-16.8999996185303,-16.1415996551514,-16.4789600372314,-16.5128002166748,-16.4538497924805,-16.4787006378174,-16.4783000946045,-16.5149993896484,-16.5149993896484,-16.4166660308838,-16.4166660308838,-17.280834197998,-17,-17,-17.3866672515869,-16.6499996185303,-16.6833324432373,-16.3333339691162,-16.5666675567627,-16.5166664123535,-16.6333332061768,-16.6666660308838,-13.6166667938232,-16.59055519104,-16.6166667938232,-16.1172218322754,-16.719165802002,-16.7291660308838,-16.6686115264893,-16.7614154815674,-16.6702785491943,-16.7855548858643,-16.508659362793,-16.508659362793,-17.2229995727539,-17.2409992218018,-17.2250003814697,-17.2129993438721,-17.2299995422363,-17.1658000946045,-17.1576995849609,-16.3955993652344,-16.508659362793,-16.3034000396729,-16.3034000396729],{"iconUrl":{"data":"data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAQAAAC1HAwCAAAAC0lEQVR4nGP6zwAAAgcBApocMXEAAAAASUVORK5CYII=","index":0},"iconWidth":1,"iconHeight":1},null,null,{"interactive":true,"draggable":false,"keyboard":true,"title":"","alt":"","zIndexOffset":0,"opacity":1,"riseOnHover":false,"riseOffset":250},null,null,null,null,["Sibassor","Ndangane diene","Koundame","Ndiafatt Sérère","Bill Bambara","Sassara","Gamboul","Joal","Ngaparou","Soubedioune","Terou Baye Sogui","Bossikang","Bossikang","Betenti","Foundiougne","Ndakhouga","Cayar","Mballing","Pointe Sarene","Nianigue","Mboro sur Mer","Yene Guedji","Yene Told","Yene Kao","yene kelle","Toubab Dialao","Ndangane diene","Fayako","Felir","Abene","Ngaparou","Ngaparou","Ngaparou","Potou","Sare Dao","Lompoul/Mer","Mbour","Guereo","Ndayane","Popenguine","Tassinere","Tassinere","Pilote Gandiol","Pilote Gandiol","Ngadior","Mounde","Missirah","Ngadior","Djirnack Barra","Sandicoly","Sourou","Sipo","Medina Sangako","Soucouta","Toubacouta","Moumbaye","Moumbaye","Mouit","Mouit","Ndeppe 1","Ouakam","Ngor","Hann","Fimela","Ndangane Sambou","Djilor Djidiack","Faoye","Sakhor","Simal","Iles de Mar","Thialane","Bassoul","Bassar","Palmarin Ngalou","Niodior","Dionewar","Falia","Djifere","Boudiediete","Diembéring","Goxu Mbacc","Goxu Mbacc","Gouye Dioulancar","Khembé","Ngadié","Sendou","Minam","Nditakh","Nianghal","Elinkine","Goxu Mbacc","Guet Ndar","Guet Ndar"],{"interactive":false,"permanent":true,"direction":"top","opacity":1,"offset":[0,0],"textsize":"10px","textOnly":true,"className":"","sticky":true},null]}],"limits":{"lat":[12.3030996322632,16.5533332824707],"lng":[-17.4580554962158,-13.6166667938232]}},"evals":[],"jsHooks":[]}</script>
```


Mode d’enregistrement et bancarisation des données collectées

Traitement des données


# Résultats 

## 	Suivi périodique des effectifs




Nombre d’unités de pêche (total et de différentes catégories), par région, par pays

### Nombre d’unités de pêche (total et de différentes catégories), par région, par pays



```r
liste.sites.init %>% inner_join(donnees$effectif_denombre) %>% 
  mutate(groupe_engin=substr(type_engin,1,4)) %>% 
  group_by(groupe_engin,code_strate,type_pirogue) %>% 
  dplyr::summarise(Equipage_moyen=mean(nb_equipage),Nbre_embar=sum(nb_unites))%>% 
  kbl() %>% kable_styling()
```

```
## Joining, by = c("id_site", "numero_fiche", "date_enquete", "pays", "nom_site")
## `summarise()` has grouped output by 'groupe_engin', 'code_strate'. You can
## override using the `.groups` argument.
```

<table class="table" style="margin-left: auto; margin-right: auto;">
 <thead>
  <tr>
   <th style="text-align:left;"> groupe_engin </th>
   <th style="text-align:left;"> code_strate </th>
   <th style="text-align:left;"> type_pirogue </th>
   <th style="text-align:right;"> Equipage_moyen </th>
   <th style="text-align:right;"> Nbre_embar </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:left;"> AU </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:left;"> MEM </td>
   <td style="text-align:right;"> 3.666667 </td>
   <td style="text-align:right;"> 956 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> AU </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:left;"> MON </td>
   <td style="text-align:right;"> 2.000000 </td>
   <td style="text-align:right;"> 4 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> AU </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:left;"> PIE </td>
   <td style="text-align:right;"> 2.000000 </td>
   <td style="text-align:right;"> 0 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> EP </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:left;"> MEM </td>
   <td style="text-align:right;"> 1.857143 </td>
   <td style="text-align:right;"> 143 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> EP </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:left;"> MOA </td>
   <td style="text-align:right;"> 2.000000 </td>
   <td style="text-align:right;"> 22 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> EP </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:left;"> PIE </td>
   <td style="text-align:right;"> 1.400000 </td>
   <td style="text-align:right;"> 43 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> FMCF </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:left;"> FIB </td>
   <td style="text-align:right;"> 2.000000 </td>
   <td style="text-align:right;"> 50 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> FMCF </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:left;"> MEM </td>
   <td style="text-align:right;"> 4.423077 </td>
   <td style="text-align:right;"> 3923 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> FMCF </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:left;"> MOA </td>
   <td style="text-align:right;"> 3.000000 </td>
   <td style="text-align:right;"> 11 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> FMCS </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:left;"> FIB </td>
   <td style="text-align:right;"> 8.000000 </td>
   <td style="text-align:right;"> 2 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> FMCS </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:left;"> MEM </td>
   <td style="text-align:right;"> 6.562500 </td>
   <td style="text-align:right;"> 336 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> FMCS </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:left;"> MOA </td>
   <td style="text-align:right;"> 1.500000 </td>
   <td style="text-align:right;"> 3 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> FMD </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:left;"> MEM </td>
   <td style="text-align:right;"> 5.241936 </td>
   <td style="text-align:right;"> 2514 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> FMD </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:left;"> MOA </td>
   <td style="text-align:right;"> 3.857143 </td>
   <td style="text-align:right;"> 110 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> FME </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:left;"> MEM </td>
   <td style="text-align:right;"> 8.379310 </td>
   <td style="text-align:right;"> 552 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> FME </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:left;"> MOA </td>
   <td style="text-align:right;"> 4.000000 </td>
   <td style="text-align:right;"> 8 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> FS </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:left;"> MEM </td>
   <td style="text-align:right;"> 8.000000 </td>
   <td style="text-align:right;"> 3 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> LI </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:left;"> FIB </td>
   <td style="text-align:right;"> 3.250000 </td>
   <td style="text-align:right;"> 326 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> LI </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:left;"> MEM </td>
   <td style="text-align:right;"> 3.060606 </td>
   <td style="text-align:right;"> 4004 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> LI </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:left;"> MOA </td>
   <td style="text-align:right;"> 2.000000 </td>
   <td style="text-align:right;"> 3 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> PA </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:left;"> FIB </td>
   <td style="text-align:right;"> 5.000000 </td>
   <td style="text-align:right;"> 77 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> PA </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:left;"> MEM </td>
   <td style="text-align:right;"> 3.875000 </td>
   <td style="text-align:right;"> 1259 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> PA </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:left;"> MOA </td>
   <td style="text-align:right;"> 2.000000 </td>
   <td style="text-align:right;"> 3 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> PA </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:left;"> PIE </td>
   <td style="text-align:right;"> 1.000000 </td>
   <td style="text-align:right;"> 3 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> PIE </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:left;"> MEM </td>
   <td style="text-align:right;"> 4.076923 </td>
   <td style="text-align:right;"> 443 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> PIE </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:left;"> PIE </td>
   <td style="text-align:right;"> 6.000000 </td>
   <td style="text-align:right;"> 1 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> SP </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:left;"> FIB </td>
   <td style="text-align:right;"> 30.000000 </td>
   <td style="text-align:right;"> 500 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> SP </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:left;"> MEM </td>
   <td style="text-align:right;"> 16.157895 </td>
   <td style="text-align:right;"> 140 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> ST </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:left;"> FIB </td>
   <td style="text-align:right;"> 30.000000 </td>
   <td style="text-align:right;"> 500 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> ST </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:left;"> MEM </td>
   <td style="text-align:right;"> 30.642857 </td>
   <td style="text-align:right;"> 979 </td>
  </tr>
</tbody>
</table>


```r
liste.sites.init %>% inner_join(donnees$effectif_denombre) %>% 
  mutate(groupe_engin=substr(type_engin,1,4)) %>% 
  group_by(groupe_engin,niveau_adm1) %>% 
  dplyr::summarise(Nbre_embar=sum(nb_unites))%>% 
  ggplot()+geom_bar(aes(x=niveau_adm1,y=Nbre_embar,fill=groupe_engin),stat='identity')
```

```
## Joining, by = c("id_site", "numero_fiche", "date_enquete", "pays", "nom_site")
## `summarise()` has grouped output by 'groupe_engin'. You can override using the
## `.groups` argument.
```

<img src="/tmp/RtmpItTkgh/preview-2658e22e89d0f4.dir/Rapports_base2_files/figure-html/unnamed-chunk-5-1.png" width="672" />

```r
liste.sites.init  %>% inner_join(donnees$effectif_denombre) %>% 
  mutate(groupe_engin=substr(type_engin,1,4)) %>% 
  group_by(type_pirogue,niveau_adm1) %>% 
  dplyr::summarise(Nbre_embar=sum(nb_unites))%>% 
  ggplot()+geom_bar(aes(x=niveau_adm1,y=Nbre_embar,fill=type_pirogue),stat='identity')
```

```
## Joining, by = c("id_site", "numero_fiche", "date_enquete", "pays", "nom_site")
## `summarise()` has grouped output by 'type_pirogue'. You can override using the
## `.groups` argument.
```

<img src="/tmp/RtmpItTkgh/preview-2658e22e89d0f4.dir/Rapports_base2_files/figure-html/unnamed-chunk-5-2.png" width="672" />



###  Taux de motorisation des U.P. (total et par catégorie d’U.P.), par région, par pays




```r
liste.sites.init %>% inner_join(donnees$effectif_denombre) %>%
  mutate(groupe_engin=substr(type_engin,1,4)) %>%
  group_by(groupe_engin,motorisation) %>%
  dplyr::summarise(Nbre_embar=sum(nb_unites))->motor1
```

```
## Joining, by = c("id_site", "numero_fiche", "date_enquete", "pays", "nom_site")
## `summarise()` has grouped output by 'groupe_engin'. You can override using the
## `.groups` argument.
```

```r
motor1 %>% dplyr::group_by(groupe_engin) %>% dplyr::summarise(nb.total=sum(Nbre_embar)) %>%
  inner_join(motor1) %>% mutate(Pourcent=100*Nbre_embar/nb.total) %>%
  ggplot()+geom_bar(aes(x=groupe_engin,fill=motorisation,y=Pourcent),stat='identity')
```

```
## Joining, by = "groupe_engin"
```

<img src="/tmp/RtmpItTkgh/preview-2658e22e89d0f4.dir/Rapports_base2_files/figure-html/unnamed-chunk-6-1.png" width="672" />

```r
liste.sites.init  %>% inner_join(donnees$effectif_denombre) %>%
  mutate(groupe_engin=substr(type_engin,1,4)) %>%
  group_by(niveau_adm1 ,groupe_engin,motorisation) %>%
  dplyr::summarise(Nbre_embar=sum(nb_unites))->motor1.reg
```

```
## Joining, by = c("id_site", "numero_fiche", "date_enquete", "pays", "nom_site")
## `summarise()` has grouped output by 'niveau_adm1', 'groupe_engin'. You can
## override using the `.groups` argument.
```

```r
motor1.reg %>% dplyr::group_by(groupe_engin,niveau_adm1) %>% dplyr::summarise(nb.total=sum(Nbre_embar)) %>%
  inner_join(motor1.reg) %>% mutate(Pourcent=100*Nbre_embar/nb.total) %>%
  ggplot()+geom_bar(aes(x=groupe_engin,fill=motorisation,y=Pourcent),stat='identity')+
  facet_wrap(~niveau_adm1)+
   theme(axis.text.x = element_text(angle = 90, vjust = 0.5, hjust=1))
```

```
## `summarise()` has grouped output by 'groupe_engin'. You can override using the
## `.groups` argument.
## Joining, by = c("groupe_engin", "niveau_adm1")
```

```
## Warning: Removed 1 rows containing missing values (`position_stack()`).
```

<img src="/tmp/RtmpItTkgh/preview-2658e22e89d0f4.dir/Rapports_base2_files/figure-html/unnamed-chunk-6-2.png" width="672" />

###  Nombre de pêcheurs (valeur approchée par les moyennes par catégorie d’U.P.), par région, par pays


```r
liste.sites.init  %>% inner_join(donnees$effectif_denombre) %>%
  mutate(groupe_engin=substr(type_engin,1,4)) %>%
  group_by(groupe_engin) %>%
  dplyr::summarise(Equipage_moyen=mean(nb_equipage),Nbre_embar=sum(nb_unites))%>%
  mutate(nb.pecheur=Nbre_embar*Equipage_moyen)->nb.pecheurs
```

```
## Joining, by = c("id_site", "numero_fiche", "date_enquete", "pays", "nom_site")
```

```r
nb.pecheurs %>%  kbl() %>% kable_styling()
```

<table class="table" style="margin-left: auto; margin-right: auto;">
 <thead>
  <tr>
   <th style="text-align:left;"> groupe_engin </th>
   <th style="text-align:right;"> Equipage_moyen </th>
   <th style="text-align:right;"> Nbre_embar </th>
   <th style="text-align:right;"> nb.pecheur </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:left;"> AU </td>
   <td style="text-align:right;"> 3.470588 </td>
   <td style="text-align:right;"> 960 </td>
   <td style="text-align:right;"> 3331.765 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> EP </td>
   <td style="text-align:right;"> 1.750000 </td>
   <td style="text-align:right;"> 208 </td>
   <td style="text-align:right;"> 364.000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> FMCF </td>
   <td style="text-align:right;"> 4.351852 </td>
   <td style="text-align:right;"> 3984 </td>
   <td style="text-align:right;"> 17337.778 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> FMCS </td>
   <td style="text-align:right;"> 6.105263 </td>
   <td style="text-align:right;"> 341 </td>
   <td style="text-align:right;"> 2081.895 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> FMD </td>
   <td style="text-align:right;"> 5.101449 </td>
   <td style="text-align:right;"> 2624 </td>
   <td style="text-align:right;"> 13386.203 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> FME </td>
   <td style="text-align:right;"> 8.233333 </td>
   <td style="text-align:right;"> 560 </td>
   <td style="text-align:right;"> 4610.667 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> FS </td>
   <td style="text-align:right;"> 8.000000 </td>
   <td style="text-align:right;"> 3 </td>
   <td style="text-align:right;"> 24.000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> LI </td>
   <td style="text-align:right;"> 3.052632 </td>
   <td style="text-align:right;"> 4333 </td>
   <td style="text-align:right;"> 13227.053 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> PA </td>
   <td style="text-align:right;"> 3.750000 </td>
   <td style="text-align:right;"> 1342 </td>
   <td style="text-align:right;"> 5032.500 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> PIE </td>
   <td style="text-align:right;"> 4.214286 </td>
   <td style="text-align:right;"> 444 </td>
   <td style="text-align:right;"> 1871.143 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> SP </td>
   <td style="text-align:right;"> 16.850000 </td>
   <td style="text-align:right;"> 640 </td>
   <td style="text-align:right;"> 10784.000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> ST </td>
   <td style="text-align:right;"> 30.600000 </td>
   <td style="text-align:right;"> 1479 </td>
   <td style="text-align:right;"> 45257.400 </td>
  </tr>
</tbody>
</table>

```r
nb.pecheurs %>% dplyr::summarise(nb.total=sum(nb.pecheur)) %>%
 kbl() %>% kable_styling()
```

<table class="table" style="margin-left: auto; margin-right: auto;">
 <thead>
  <tr>
   <th style="text-align:right;"> nb.total </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:right;"> 117308.4 </td>
  </tr>
</tbody>
</table>

Calcul par région


```r
liste.sites.init  %>% inner_join(donnees$effectif_denombre) %>%
  mutate(groupe_engin=substr(type_engin,1,4)) %>%
  group_by(groupe_engin,niveau_adm1) %>%
  dplyr::summarise(Equipage_moyen=mean(nb_equipage),Nbre_embar=sum(nb_unites))%>%
  mutate(nb.pecheur=Nbre_embar*Equipage_moyen)->nb.pecheurs.reg
```

```
## Joining, by = c("id_site", "numero_fiche", "date_enquete", "pays", "nom_site")
## `summarise()` has grouped output by 'groupe_engin'. You can override using the
## `.groups` argument.
```

```r
nb.pecheurs.reg %>%  kbl() %>% kable_styling()
```

<table class="table" style="margin-left: auto; margin-right: auto;">
 <thead>
  <tr>
   <th style="text-align:left;"> groupe_engin </th>
   <th style="text-align:left;"> niveau_adm1 </th>
   <th style="text-align:right;"> Equipage_moyen </th>
   <th style="text-align:right;"> Nbre_embar </th>
   <th style="text-align:right;"> nb.pecheur </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:left;"> AU </td>
   <td style="text-align:left;"> Dakar </td>
   <td style="text-align:right;"> 5.400000 </td>
   <td style="text-align:right;"> 511 </td>
   <td style="text-align:right;"> 2759.40000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> AU </td>
   <td style="text-align:left;"> Fatick </td>
   <td style="text-align:right;"> 3.500000 </td>
   <td style="text-align:right;"> 63 </td>
   <td style="text-align:right;"> 220.50000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> AU </td>
   <td style="text-align:left;"> Saint Louis </td>
   <td style="text-align:right;"> 3.000000 </td>
   <td style="text-align:right;"> 2 </td>
   <td style="text-align:right;"> 6.00000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> AU </td>
   <td style="text-align:left;"> Ziguinchor </td>
   <td style="text-align:right;"> 2.000000 </td>
   <td style="text-align:right;"> 384 </td>
   <td style="text-align:right;"> 768.00000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> EP </td>
   <td style="text-align:left;"> Dakar </td>
   <td style="text-align:right;"> 2.000000 </td>
   <td style="text-align:right;"> 20 </td>
   <td style="text-align:right;"> 40.00000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> EP </td>
   <td style="text-align:left;"> Fatick </td>
   <td style="text-align:right;"> 2.000000 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 0.00000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> EP </td>
   <td style="text-align:left;"> Kaolack </td>
   <td style="text-align:right;"> 1.333333 </td>
   <td style="text-align:right;"> 16 </td>
   <td style="text-align:right;"> 21.33333 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> EP </td>
   <td style="text-align:left;"> Thies </td>
   <td style="text-align:right;"> 1.000000 </td>
   <td style="text-align:right;"> 10 </td>
   <td style="text-align:right;"> 10.00000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> EP </td>
   <td style="text-align:left;"> ziguinchor </td>
   <td style="text-align:right;"> 2.000000 </td>
   <td style="text-align:right;"> 10 </td>
   <td style="text-align:right;"> 20.00000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> EP </td>
   <td style="text-align:left;"> Ziguinchor </td>
   <td style="text-align:right;"> 1.888889 </td>
   <td style="text-align:right;"> 152 </td>
   <td style="text-align:right;"> 287.11111 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> FMCF </td>
   <td style="text-align:left;"> Dakar </td>
   <td style="text-align:right;"> 6.125000 </td>
   <td style="text-align:right;"> 41 </td>
   <td style="text-align:right;"> 251.12500 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> FMCF </td>
   <td style="text-align:left;"> Fatick </td>
   <td style="text-align:right;"> 4.888889 </td>
   <td style="text-align:right;"> 199 </td>
   <td style="text-align:right;"> 972.88889 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> FMCF </td>
   <td style="text-align:left;"> Louga </td>
   <td style="text-align:right;"> 6.000000 </td>
   <td style="text-align:right;"> 311 </td>
   <td style="text-align:right;"> 1866.00000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> FMCF </td>
   <td style="text-align:left;"> Saint Louis </td>
   <td style="text-align:right;"> 4.500000 </td>
   <td style="text-align:right;"> 2007 </td>
   <td style="text-align:right;"> 9031.50000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> FMCF </td>
   <td style="text-align:left;"> Thies </td>
   <td style="text-align:right;"> 3.083333 </td>
   <td style="text-align:right;"> 947 </td>
   <td style="text-align:right;"> 2919.91667 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> FMCF </td>
   <td style="text-align:left;"> ziguinchor </td>
   <td style="text-align:right;"> 3.000000 </td>
   <td style="text-align:right;"> 11 </td>
   <td style="text-align:right;"> 33.00000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> FMCF </td>
   <td style="text-align:left;"> Ziguinchor </td>
   <td style="text-align:right;"> 3.545454 </td>
   <td style="text-align:right;"> 468 </td>
   <td style="text-align:right;"> 1659.27273 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> FMCS </td>
   <td style="text-align:left;"> Dakar </td>
   <td style="text-align:right;"> 8.000000 </td>
   <td style="text-align:right;"> 144 </td>
   <td style="text-align:right;"> 1152.00000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> FMCS </td>
   <td style="text-align:left;"> Fatick </td>
   <td style="text-align:right;"> 5.500000 </td>
   <td style="text-align:right;"> 121 </td>
   <td style="text-align:right;"> 665.50000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> FMCS </td>
   <td style="text-align:left;"> Kaolack </td>
   <td style="text-align:right;"> 2.000000 </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 2.00000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> FMCS </td>
   <td style="text-align:left;"> Thies </td>
   <td style="text-align:right;"> 2.000000 </td>
   <td style="text-align:right;"> 14 </td>
   <td style="text-align:right;"> 28.00000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> FMCS </td>
   <td style="text-align:left;"> Ziguinchor </td>
   <td style="text-align:right;"> 5.750000 </td>
   <td style="text-align:right;"> 61 </td>
   <td style="text-align:right;"> 350.75000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> FMD </td>
   <td style="text-align:left;"> Dakar </td>
   <td style="text-align:right;"> 8.000000 </td>
   <td style="text-align:right;"> 446 </td>
   <td style="text-align:right;"> 3568.00000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> FMD </td>
   <td style="text-align:left;"> fatick </td>
   <td style="text-align:right;"> 8.000000 </td>
   <td style="text-align:right;"> 8 </td>
   <td style="text-align:right;"> 64.00000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> FMD </td>
   <td style="text-align:left;"> Fatick </td>
   <td style="text-align:right;"> 5.473684 </td>
   <td style="text-align:right;"> 261 </td>
   <td style="text-align:right;"> 1428.63158 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> FMD </td>
   <td style="text-align:left;"> Kaolack </td>
   <td style="text-align:right;"> 3.222222 </td>
   <td style="text-align:right;"> 77 </td>
   <td style="text-align:right;"> 248.11111 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> FMD </td>
   <td style="text-align:left;"> Louga </td>
   <td style="text-align:right;"> 6.666667 </td>
   <td style="text-align:right;"> 341 </td>
   <td style="text-align:right;"> 2273.33333 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> FMD </td>
   <td style="text-align:left;"> Saint Louis </td>
   <td style="text-align:right;"> 3.363636 </td>
   <td style="text-align:right;"> 440 </td>
   <td style="text-align:right;"> 1480.00000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> FMD </td>
   <td style="text-align:left;"> Thies </td>
   <td style="text-align:right;"> 7.333333 </td>
   <td style="text-align:right;"> 303 </td>
   <td style="text-align:right;"> 2222.00000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> FMD </td>
   <td style="text-align:left;"> ziguinchor </td>
   <td style="text-align:right;"> 4.000000 </td>
   <td style="text-align:right;"> 47 </td>
   <td style="text-align:right;"> 188.00000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> FMD </td>
   <td style="text-align:left;"> Ziguinchor </td>
   <td style="text-align:right;"> 4.461538 </td>
   <td style="text-align:right;"> 701 </td>
   <td style="text-align:right;"> 3127.53846 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> FME </td>
   <td style="text-align:left;"> Dakar </td>
   <td style="text-align:right;"> 11.666667 </td>
   <td style="text-align:right;"> 98 </td>
   <td style="text-align:right;"> 1143.33333 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> FME </td>
   <td style="text-align:left;"> Fatick </td>
   <td style="text-align:right;"> 8.866667 </td>
   <td style="text-align:right;"> 216 </td>
   <td style="text-align:right;"> 1915.20000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> FME </td>
   <td style="text-align:left;"> Kaolack </td>
   <td style="text-align:right;"> 4.000000 </td>
   <td style="text-align:right;"> 4 </td>
   <td style="text-align:right;"> 16.00000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> FME </td>
   <td style="text-align:left;"> Thies </td>
   <td style="text-align:right;"> 11.000000 </td>
   <td style="text-align:right;"> 41 </td>
   <td style="text-align:right;"> 451.00000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> FME </td>
   <td style="text-align:left;"> ziguinchor </td>
   <td style="text-align:right;"> 4.000000 </td>
   <td style="text-align:right;"> 8 </td>
   <td style="text-align:right;"> 32.00000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> FME </td>
   <td style="text-align:left;"> Ziguinchor </td>
   <td style="text-align:right;"> 2.800000 </td>
   <td style="text-align:right;"> 193 </td>
   <td style="text-align:right;"> 540.40000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> FS </td>
   <td style="text-align:left;"> Ziguinchor </td>
   <td style="text-align:right;"> 8.000000 </td>
   <td style="text-align:right;"> 3 </td>
   <td style="text-align:right;"> 24.00000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> LI </td>
   <td style="text-align:left;"> Dakar </td>
   <td style="text-align:right;"> 3.466667 </td>
   <td style="text-align:right;"> 1707 </td>
   <td style="text-align:right;"> 5917.60000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> LI </td>
   <td style="text-align:left;"> Fatick </td>
   <td style="text-align:right;"> 2.400000 </td>
   <td style="text-align:right;"> 27 </td>
   <td style="text-align:right;"> 64.80000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> LI </td>
   <td style="text-align:left;"> Kaolack </td>
   <td style="text-align:right;"> 2.000000 </td>
   <td style="text-align:right;"> 3 </td>
   <td style="text-align:right;"> 6.00000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> LI </td>
   <td style="text-align:left;"> Saint Louis </td>
   <td style="text-align:right;"> 3.666667 </td>
   <td style="text-align:right;"> 231 </td>
   <td style="text-align:right;"> 847.00000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> LI </td>
   <td style="text-align:left;"> Thies </td>
   <td style="text-align:right;"> 3.000000 </td>
   <td style="text-align:right;"> 2351 </td>
   <td style="text-align:right;"> 7053.00000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> LI </td>
   <td style="text-align:left;"> Ziguinchor </td>
   <td style="text-align:right;"> 2.000000 </td>
   <td style="text-align:right;"> 14 </td>
   <td style="text-align:right;"> 28.00000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> PA </td>
   <td style="text-align:left;"> Dakar </td>
   <td style="text-align:right;"> 4.333333 </td>
   <td style="text-align:right;"> 240 </td>
   <td style="text-align:right;"> 1040.00000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> PA </td>
   <td style="text-align:left;"> Saint Louis </td>
   <td style="text-align:right;"> 3.800000 </td>
   <td style="text-align:right;"> 80 </td>
   <td style="text-align:right;"> 304.00000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> PA </td>
   <td style="text-align:left;"> Thies </td>
   <td style="text-align:right;"> 4.000000 </td>
   <td style="text-align:right;"> 680 </td>
   <td style="text-align:right;"> 2720.00000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> PA </td>
   <td style="text-align:left;"> Ziguinchor </td>
   <td style="text-align:right;"> 3.000000 </td>
   <td style="text-align:right;"> 342 </td>
   <td style="text-align:right;"> 1026.00000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> PIE </td>
   <td style="text-align:left;"> Fatick </td>
   <td style="text-align:right;"> 3.333333 </td>
   <td style="text-align:right;"> 155 </td>
   <td style="text-align:right;"> 516.66667 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> PIE </td>
   <td style="text-align:left;"> Saint Louis </td>
   <td style="text-align:right;"> 5.000000 </td>
   <td style="text-align:right;"> 16 </td>
   <td style="text-align:right;"> 80.00000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> PIE </td>
   <td style="text-align:left;"> Thies </td>
   <td style="text-align:right;"> 4.166667 </td>
   <td style="text-align:right;"> 243 </td>
   <td style="text-align:right;"> 1012.50000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> PIE </td>
   <td style="text-align:left;"> Ziguinchor </td>
   <td style="text-align:right;"> 4.000000 </td>
   <td style="text-align:right;"> 30 </td>
   <td style="text-align:right;"> 120.00000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> SP </td>
   <td style="text-align:left;"> Dakar </td>
   <td style="text-align:right;"> 27.500000 </td>
   <td style="text-align:right;"> 14 </td>
   <td style="text-align:right;"> 385.00000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> SP </td>
   <td style="text-align:left;"> Fatick </td>
   <td style="text-align:right;"> 16.000000 </td>
   <td style="text-align:right;"> 107 </td>
   <td style="text-align:right;"> 1712.00000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> SP </td>
   <td style="text-align:left;"> Kaolack </td>
   <td style="text-align:right;"> 8.000000 </td>
   <td style="text-align:right;"> 9 </td>
   <td style="text-align:right;"> 72.00000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> SP </td>
   <td style="text-align:left;"> Saint Louis </td>
   <td style="text-align:right;"> 30.000000 </td>
   <td style="text-align:right;"> 500 </td>
   <td style="text-align:right;"> 15000.00000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> SP </td>
   <td style="text-align:left;"> Thies </td>
   <td style="text-align:right;"> 15.000000 </td>
   <td style="text-align:right;"> 3 </td>
   <td style="text-align:right;"> 45.00000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> SP </td>
   <td style="text-align:left;"> Ziguinchor </td>
   <td style="text-align:right;"> 6.000000 </td>
   <td style="text-align:right;"> 7 </td>
   <td style="text-align:right;"> 42.00000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> ST </td>
   <td style="text-align:left;"> Dakar </td>
   <td style="text-align:right;"> 29.875000 </td>
   <td style="text-align:right;"> 312 </td>
   <td style="text-align:right;"> 9321.00000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> ST </td>
   <td style="text-align:left;"> Fatick </td>
   <td style="text-align:right;"> 15.000000 </td>
   <td style="text-align:right;"> 15 </td>
   <td style="text-align:right;"> 225.00000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> ST </td>
   <td style="text-align:left;"> Saint Louis </td>
   <td style="text-align:right;"> 30.000000 </td>
   <td style="text-align:right;"> 500 </td>
   <td style="text-align:right;"> 15000.00000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> ST </td>
   <td style="text-align:left;"> Thies </td>
   <td style="text-align:right;"> 38.333333 </td>
   <td style="text-align:right;"> 639 </td>
   <td style="text-align:right;"> 24495.00000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> ST </td>
   <td style="text-align:left;"> Ziguinchor </td>
   <td style="text-align:right;"> 30.000000 </td>
   <td style="text-align:right;"> 13 </td>
   <td style="text-align:right;"> 390.00000 </td>
  </tr>
</tbody>
</table>

```r
nb.pecheurs.reg %>% group_by(niveau_adm1) %>% dplyr::summarise(nb.total=sum(nb.pecheur)) %>%
 kbl() %>% kable_styling()
```

<table class="table" style="margin-left: auto; margin-right: auto;">
 <thead>
  <tr>
   <th style="text-align:left;"> niveau_adm1 </th>
   <th style="text-align:right;"> nb.total </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:left;"> Dakar </td>
   <td style="text-align:right;"> 25577.4583 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> fatick </td>
   <td style="text-align:right;"> 64.0000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Fatick </td>
   <td style="text-align:right;"> 7721.1871 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Kaolack </td>
   <td style="text-align:right;"> 365.4444 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Louga </td>
   <td style="text-align:right;"> 4139.3333 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Saint Louis </td>
   <td style="text-align:right;"> 41748.5000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Thies </td>
   <td style="text-align:right;"> 40956.4167 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> ziguinchor </td>
   <td style="text-align:right;"> 273.0000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Ziguinchor </td>
   <td style="text-align:right;"> 8363.0723 </td>
  </tr>
</tbody>
</table>

```r
liste.sites.init %>% inner_join(donnees$effectif_denombre) %>%
  mutate(groupe_engin=substr(type_engin,1,4)) %>%
  select(niveau_adm1,groupe_engin,nb_equipage) %>%
  ggplot()+geom_point(aes(x=niveau_adm1,y=nb_equipage))+
  facet_wrap(~groupe_engin)+
   theme(axis.text.x = element_text(angle = 90, vjust = 0.5, hjust=1))+
  ggtitle("Valeurs équipage moyen par groupe d'engin et région")
```

```
## Joining, by = c("id_site", "numero_fiche", "date_enquete", "pays", "nom_site")
```

<img src="/tmp/RtmpItTkgh/preview-2658e22e89d0f4.dir/Rapports_base2_files/figure-html/unnamed-chunk-8-1.png" width="672" />

###  Nombre approximatif de professionnels de différents types, par région




```r
liste.sites.init[,c(1,13,seq(39,45))] %>%
  pivot_longer( cols = names(liste.sites.init)[c(seq(39,45))], names_to = "Operateurs") %>%
  group_by(Operateurs) %>%
  summarize(nb.operateurs=sum(value)) ->nb.ope
nb.ope %>%
  kbl() %>% kable_styling()
```

<table class="table" style="margin-left: auto; margin-right: auto;">
 <thead>
  <tr>
   <th style="text-align:left;"> Operateurs </th>
   <th style="text-align:right;"> nb.operateurs </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:left;"> nb_ecailleurs </td>
   <td style="text-align:right;"> 954 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> nb_marey </td>
   <td style="text-align:right;"> 1482 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> nb_micro_marey </td>
   <td style="text-align:right;"> 5363 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> nb_pointeurs </td>
   <td style="text-align:right;"> 1059 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> nb_porteurs </td>
   <td style="text-align:right;"> 1997 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> nb_transfo </td>
   <td style="text-align:right;"> 5048 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> nb_usine </td>
   <td style="text-align:right;"> 186 </td>
  </tr>
</tbody>
</table>

```r
ggplot(nb.ope)+geom_bar(aes(x=Operateurs,y=nb.operateurs),stat='identity')
```

<img src="/tmp/RtmpItTkgh/preview-2658e22e89d0f4.dir/Rapports_base2_files/figure-html/unnamed-chunk-9-1.png" width="672" />

```r
liste.sites.init[,c(1,13,seq(39,45))] %>%
  pivot_longer( cols = names(liste.sites.init)[c(seq(39,45))], names_to = "Operateurs") %>%
  group_by(niveau_adm1,Operateurs) %>%
  summarize(nb.operateurs=sum(value)) ->nb.ope.reg
```

```
## `summarise()` has grouped output by 'niveau_adm1'. You can override using the
## `.groups` argument.
```

```r
nb.ope.reg%>%
  kbl() %>% kable_styling()
```

<table class="table" style="margin-left: auto; margin-right: auto;">
 <thead>
  <tr>
   <th style="text-align:left;"> niveau_adm1 </th>
   <th style="text-align:left;"> Operateurs </th>
   <th style="text-align:right;"> nb.operateurs </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:left;"> Dakar </td>
   <td style="text-align:left;"> nb_ecailleurs </td>
   <td style="text-align:right;"> 251 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Dakar </td>
   <td style="text-align:left;"> nb_marey </td>
   <td style="text-align:right;"> 376 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Dakar </td>
   <td style="text-align:left;"> nb_micro_marey </td>
   <td style="text-align:right;"> 917 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Dakar </td>
   <td style="text-align:left;"> nb_pointeurs </td>
   <td style="text-align:right;"> 263 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Dakar </td>
   <td style="text-align:left;"> nb_porteurs </td>
   <td style="text-align:right;"> 369 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Dakar </td>
   <td style="text-align:left;"> nb_transfo </td>
   <td style="text-align:right;"> 1077 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Dakar </td>
   <td style="text-align:left;"> nb_usine </td>
   <td style="text-align:right;"> 114 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> fatick </td>
   <td style="text-align:left;"> nb_ecailleurs </td>
   <td style="text-align:right;"> 0 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> fatick </td>
   <td style="text-align:left;"> nb_marey </td>
   <td style="text-align:right;"> 2 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> fatick </td>
   <td style="text-align:left;"> nb_micro_marey </td>
   <td style="text-align:right;"> 25 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> fatick </td>
   <td style="text-align:left;"> nb_pointeurs </td>
   <td style="text-align:right;"> 0 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> fatick </td>
   <td style="text-align:left;"> nb_porteurs </td>
   <td style="text-align:right;"> 6 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> fatick </td>
   <td style="text-align:left;"> nb_transfo </td>
   <td style="text-align:right;"> 80 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> fatick </td>
   <td style="text-align:left;"> nb_usine </td>
   <td style="text-align:right;"> 0 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Fatick </td>
   <td style="text-align:left;"> nb_ecailleurs </td>
   <td style="text-align:right;"> 83 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Fatick </td>
   <td style="text-align:left;"> nb_marey </td>
   <td style="text-align:right;"> 201 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Fatick </td>
   <td style="text-align:left;"> nb_micro_marey </td>
   <td style="text-align:right;"> 720 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Fatick </td>
   <td style="text-align:left;"> nb_pointeurs </td>
   <td style="text-align:right;"> 22 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Fatick </td>
   <td style="text-align:left;"> nb_porteurs </td>
   <td style="text-align:right;"> 184 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Fatick </td>
   <td style="text-align:left;"> nb_transfo </td>
   <td style="text-align:right;"> 1199 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Fatick </td>
   <td style="text-align:left;"> nb_usine </td>
   <td style="text-align:right;"> 8 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Kaolack </td>
   <td style="text-align:left;"> nb_ecailleurs </td>
   <td style="text-align:right;"> 0 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Kaolack </td>
   <td style="text-align:left;"> nb_marey </td>
   <td style="text-align:right;"> 3 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Kaolack </td>
   <td style="text-align:left;"> nb_micro_marey </td>
   <td style="text-align:right;"> 88 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Kaolack </td>
   <td style="text-align:left;"> nb_pointeurs </td>
   <td style="text-align:right;"> 0 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Kaolack </td>
   <td style="text-align:left;"> nb_porteurs </td>
   <td style="text-align:right;"> 0 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Kaolack </td>
   <td style="text-align:left;"> nb_transfo </td>
   <td style="text-align:right;"> 0 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Kaolack </td>
   <td style="text-align:left;"> nb_usine </td>
   <td style="text-align:right;"> 0 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Louga </td>
   <td style="text-align:left;"> nb_ecailleurs </td>
   <td style="text-align:right;"> 36 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Louga </td>
   <td style="text-align:left;"> nb_marey </td>
   <td style="text-align:right;"> 13 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Louga </td>
   <td style="text-align:left;"> nb_micro_marey </td>
   <td style="text-align:right;"> 32 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Louga </td>
   <td style="text-align:left;"> nb_pointeurs </td>
   <td style="text-align:right;"> 12 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Louga </td>
   <td style="text-align:left;"> nb_porteurs </td>
   <td style="text-align:right;"> 20 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Louga </td>
   <td style="text-align:left;"> nb_transfo </td>
   <td style="text-align:right;"> 360 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Louga </td>
   <td style="text-align:left;"> nb_usine </td>
   <td style="text-align:right;"> 0 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Saint Louis </td>
   <td style="text-align:left;"> nb_ecailleurs </td>
   <td style="text-align:right;"> 108 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Saint Louis </td>
   <td style="text-align:left;"> nb_marey </td>
   <td style="text-align:right;"> 292 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Saint Louis </td>
   <td style="text-align:left;"> nb_micro_marey </td>
   <td style="text-align:right;"> 445 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Saint Louis </td>
   <td style="text-align:left;"> nb_pointeurs </td>
   <td style="text-align:right;"> 280 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Saint Louis </td>
   <td style="text-align:left;"> nb_porteurs </td>
   <td style="text-align:right;"> 553 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Saint Louis </td>
   <td style="text-align:left;"> nb_transfo </td>
   <td style="text-align:right;"> 946 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Saint Louis </td>
   <td style="text-align:left;"> nb_usine </td>
   <td style="text-align:right;"> 12 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Thies </td>
   <td style="text-align:left;"> nb_ecailleurs </td>
   <td style="text-align:right;"> 422 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Thies </td>
   <td style="text-align:left;"> nb_marey </td>
   <td style="text-align:right;"> 420 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Thies </td>
   <td style="text-align:left;"> nb_micro_marey </td>
   <td style="text-align:right;"> 2737 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Thies </td>
   <td style="text-align:left;"> nb_pointeurs </td>
   <td style="text-align:right;"> 368 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Thies </td>
   <td style="text-align:left;"> nb_porteurs </td>
   <td style="text-align:right;"> 405 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Thies </td>
   <td style="text-align:left;"> nb_transfo </td>
   <td style="text-align:right;"> 813 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Thies </td>
   <td style="text-align:left;"> nb_usine </td>
   <td style="text-align:right;"> 48 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> ziguinchor </td>
   <td style="text-align:left;"> nb_ecailleurs </td>
   <td style="text-align:right;"> 8 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> ziguinchor </td>
   <td style="text-align:left;"> nb_marey </td>
   <td style="text-align:right;"> 10 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> ziguinchor </td>
   <td style="text-align:left;"> nb_micro_marey </td>
   <td style="text-align:right;"> 7 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> ziguinchor </td>
   <td style="text-align:left;"> nb_pointeurs </td>
   <td style="text-align:right;"> 5 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> ziguinchor </td>
   <td style="text-align:left;"> nb_porteurs </td>
   <td style="text-align:right;"> 5 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> ziguinchor </td>
   <td style="text-align:left;"> nb_transfo </td>
   <td style="text-align:right;"> 0 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> ziguinchor </td>
   <td style="text-align:left;"> nb_usine </td>
   <td style="text-align:right;"> 1 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Ziguinchor </td>
   <td style="text-align:left;"> nb_ecailleurs </td>
   <td style="text-align:right;"> 46 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Ziguinchor </td>
   <td style="text-align:left;"> nb_marey </td>
   <td style="text-align:right;"> 165 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Ziguinchor </td>
   <td style="text-align:left;"> nb_micro_marey </td>
   <td style="text-align:right;"> 392 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Ziguinchor </td>
   <td style="text-align:left;"> nb_pointeurs </td>
   <td style="text-align:right;"> 109 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Ziguinchor </td>
   <td style="text-align:left;"> nb_porteurs </td>
   <td style="text-align:right;"> 455 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Ziguinchor </td>
   <td style="text-align:left;"> nb_transfo </td>
   <td style="text-align:right;"> 573 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Ziguinchor </td>
   <td style="text-align:left;"> nb_usine </td>
   <td style="text-align:right;"> 3 </td>
  </tr>
</tbody>
</table>

```r
ggplot(nb.ope.reg)+geom_bar(aes(x=Operateurs,y=nb.operateurs),stat='identity')+
  facet_wrap(~niveau_adm1)+
   theme(axis.text.x = element_text(angle = 90, vjust = 0.5, hjust=1))+
  ggtitle("Nombre d'opérateur région")
```

<img src="/tmp/RtmpItTkgh/preview-2658e22e89d0f4.dir/Rapports_base2_files/figure-html/unnamed-chunk-9-2.png" width="672" />

###  Nombre (ou %age) de sites disposant de tel ou tel type de commodités, par région, par pays




```r
total<-dim(liste.sites.init)[1]

liste.sites.init[,c(1,13,seq(22,38))]  %>%
  pivot_longer( cols = names(liste.sites)[c(seq(22,38))], names_to = "Commodites") %>%
  filter(value=='Oui') %>%
  group_by(Commodites) %>%
  summarize(Site_debarq=n())%>%
 mutate(pour.site_debarq=Site_debarq/total*100) -> commodites

commodites %>%
  kbl() %>% kable_styling()
```

<table class="table" style="margin-left: auto; margin-right: auto;">
 <thead>
  <tr>
   <th style="text-align:left;"> Commodites </th>
   <th style="text-align:right;"> Site_debarq </th>
   <th style="text-align:right;"> pour.site_debarq </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:left;"> amenage </td>
   <td style="text-align:right;"> 25 </td>
   <td style="text-align:right;"> 21.008403 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> app_carburant </td>
   <td style="text-align:right;"> 45 </td>
   <td style="text-align:right;"> 37.815126 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> app_glace </td>
   <td style="text-align:right;"> 26 </td>
   <td style="text-align:right;"> 21.848740 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> boutique </td>
   <td style="text-align:right;"> 44 </td>
   <td style="text-align:right;"> 36.974790 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> charpentier </td>
   <td style="text-align:right;"> 48 </td>
   <td style="text-align:right;"> 40.336135 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> electricite </td>
   <td style="text-align:right;"> 57 </td>
   <td style="text-align:right;"> 47.899160 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> frigo </td>
   <td style="text-align:right;"> 8 </td>
   <td style="text-align:right;"> 6.722689 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> glaciere </td>
   <td style="text-align:right;"> 34 </td>
   <td style="text-align:right;"> 28.571429 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> halle_poisson </td>
   <td style="text-align:right;"> 23 </td>
   <td style="text-align:right;"> 19.327731 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> latrines </td>
   <td style="text-align:right;"> 34 </td>
   <td style="text-align:right;"> 28.571429 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> marche </td>
   <td style="text-align:right;"> 19 </td>
   <td style="text-align:right;"> 15.966387 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> parking </td>
   <td style="text-align:right;"> 23 </td>
   <td style="text-align:right;"> 19.327731 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> reparation_meca </td>
   <td style="text-align:right;"> 39 </td>
   <td style="text-align:right;"> 32.773109 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> robinet </td>
   <td style="text-align:right;"> 23 </td>
   <td style="text-align:right;"> 19.327731 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> sans_amenagement </td>
   <td style="text-align:right;"> 89 </td>
   <td style="text-align:right;"> 74.789916 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> site_transfo_amenage </td>
   <td style="text-align:right;"> 30 </td>
   <td style="text-align:right;"> 25.210084 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> telphone </td>
   <td style="text-align:right;"> 97 </td>
   <td style="text-align:right;"> 81.512605 </td>
  </tr>
</tbody>
</table>

```r
ggplot(commodites)+
  geom_bar(stat='identity',aes(x=Commodites,y=pour.site_debarq))+
   theme(axis.text.x = element_text(angle = 90, vjust = 0.5, hjust=1))
```

<img src="/tmp/RtmpItTkgh/preview-2658e22e89d0f4.dir/Rapports_base2_files/figure-html/unnamed-chunk-10-1.png" width="672" />
Par régions


```r
total.reg<- liste.sites.init %>% group_by(niveau_adm1) %>%
  dplyr::summarise(total.reg=n())


liste.sites.init[,c(1,13,seq(22,38))]  %>%
  pivot_longer( cols = names(liste.sites)[c(seq(22,38))], names_to = "Commodites") %>%
  filter(value=='Oui') %>%
  group_by(Commodites,niveau_adm1) %>%
  summarize(nb.sites=n()) %>% inner_join(total.reg) %>%
   dplyr::mutate(pour.site_debarq=100*nb.sites/total.reg)->commodites.reg
```

```
## `summarise()` has grouped output by 'Commodites'. You can override using the
## `.groups` argument.
## Joining, by = "niveau_adm1"
```

```r
commodites.reg %>%
  kbl() %>% kable_styling()
```

<table class="table" style="margin-left: auto; margin-right: auto;">
 <thead>
  <tr>
   <th style="text-align:left;"> Commodites </th>
   <th style="text-align:left;"> niveau_adm1 </th>
   <th style="text-align:right;"> nb.sites </th>
   <th style="text-align:right;"> total.reg </th>
   <th style="text-align:right;"> pour.site_debarq </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:left;"> amenage </td>
   <td style="text-align:left;"> Dakar </td>
   <td style="text-align:right;"> 5 </td>
   <td style="text-align:right;"> 19 </td>
   <td style="text-align:right;"> 26.315790 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> amenage </td>
   <td style="text-align:left;"> fatick </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 100.000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> amenage </td>
   <td style="text-align:left;"> Fatick </td>
   <td style="text-align:right;"> 2 </td>
   <td style="text-align:right;"> 42 </td>
   <td style="text-align:right;"> 4.761905 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> amenage </td>
   <td style="text-align:left;"> Louga </td>
   <td style="text-align:right;"> 2 </td>
   <td style="text-align:right;"> 3 </td>
   <td style="text-align:right;"> 66.666667 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> amenage </td>
   <td style="text-align:left;"> Saint Louis </td>
   <td style="text-align:right;"> 4 </td>
   <td style="text-align:right;"> 13 </td>
   <td style="text-align:right;"> 30.769231 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> amenage </td>
   <td style="text-align:left;"> Thies </td>
   <td style="text-align:right;"> 8 </td>
   <td style="text-align:right;"> 14 </td>
   <td style="text-align:right;"> 57.142857 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> amenage </td>
   <td style="text-align:left;"> Ziguinchor </td>
   <td style="text-align:right;"> 3 </td>
   <td style="text-align:right;"> 16 </td>
   <td style="text-align:right;"> 18.750000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> app_carburant </td>
   <td style="text-align:left;"> Dakar </td>
   <td style="text-align:right;"> 11 </td>
   <td style="text-align:right;"> 19 </td>
   <td style="text-align:right;"> 57.894737 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> app_carburant </td>
   <td style="text-align:left;"> Fatick </td>
   <td style="text-align:right;"> 8 </td>
   <td style="text-align:right;"> 42 </td>
   <td style="text-align:right;"> 19.047619 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> app_carburant </td>
   <td style="text-align:left;"> Kaolack </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 10 </td>
   <td style="text-align:right;"> 10.000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> app_carburant </td>
   <td style="text-align:left;"> Louga </td>
   <td style="text-align:right;"> 3 </td>
   <td style="text-align:right;"> 3 </td>
   <td style="text-align:right;"> 100.000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> app_carburant </td>
   <td style="text-align:left;"> Saint Louis </td>
   <td style="text-align:right;"> 6 </td>
   <td style="text-align:right;"> 13 </td>
   <td style="text-align:right;"> 46.153846 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> app_carburant </td>
   <td style="text-align:left;"> Thies </td>
   <td style="text-align:right;"> 11 </td>
   <td style="text-align:right;"> 14 </td>
   <td style="text-align:right;"> 78.571429 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> app_carburant </td>
   <td style="text-align:left;"> ziguinchor </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 100.000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> app_carburant </td>
   <td style="text-align:left;"> Ziguinchor </td>
   <td style="text-align:right;"> 4 </td>
   <td style="text-align:right;"> 16 </td>
   <td style="text-align:right;"> 25.000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> app_glace </td>
   <td style="text-align:left;"> Dakar </td>
   <td style="text-align:right;"> 5 </td>
   <td style="text-align:right;"> 19 </td>
   <td style="text-align:right;"> 26.315790 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> app_glace </td>
   <td style="text-align:left;"> Fatick </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 42 </td>
   <td style="text-align:right;"> 2.380952 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> app_glace </td>
   <td style="text-align:left;"> Kaolack </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 10 </td>
   <td style="text-align:right;"> 10.000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> app_glace </td>
   <td style="text-align:left;"> Saint Louis </td>
   <td style="text-align:right;"> 5 </td>
   <td style="text-align:right;"> 13 </td>
   <td style="text-align:right;"> 38.461539 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> app_glace </td>
   <td style="text-align:left;"> Thies </td>
   <td style="text-align:right;"> 10 </td>
   <td style="text-align:right;"> 14 </td>
   <td style="text-align:right;"> 71.428571 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> app_glace </td>
   <td style="text-align:left;"> ziguinchor </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 100.000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> app_glace </td>
   <td style="text-align:left;"> Ziguinchor </td>
   <td style="text-align:right;"> 3 </td>
   <td style="text-align:right;"> 16 </td>
   <td style="text-align:right;"> 18.750000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> boutique </td>
   <td style="text-align:left;"> Dakar </td>
   <td style="text-align:right;"> 13 </td>
   <td style="text-align:right;"> 19 </td>
   <td style="text-align:right;"> 68.421053 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> boutique </td>
   <td style="text-align:left;"> Fatick </td>
   <td style="text-align:right;"> 7 </td>
   <td style="text-align:right;"> 42 </td>
   <td style="text-align:right;"> 16.666667 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> boutique </td>
   <td style="text-align:left;"> Louga </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 3 </td>
   <td style="text-align:right;"> 33.333333 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> boutique </td>
   <td style="text-align:left;"> Saint Louis </td>
   <td style="text-align:right;"> 9 </td>
   <td style="text-align:right;"> 13 </td>
   <td style="text-align:right;"> 69.230769 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> boutique </td>
   <td style="text-align:left;"> Thies </td>
   <td style="text-align:right;"> 10 </td>
   <td style="text-align:right;"> 14 </td>
   <td style="text-align:right;"> 71.428571 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> boutique </td>
   <td style="text-align:left;"> Ziguinchor </td>
   <td style="text-align:right;"> 4 </td>
   <td style="text-align:right;"> 16 </td>
   <td style="text-align:right;"> 25.000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> charpentier </td>
   <td style="text-align:left;"> Dakar </td>
   <td style="text-align:right;"> 14 </td>
   <td style="text-align:right;"> 19 </td>
   <td style="text-align:right;"> 73.684211 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> charpentier </td>
   <td style="text-align:left;"> fatick </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 100.000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> charpentier </td>
   <td style="text-align:left;"> Fatick </td>
   <td style="text-align:right;"> 19 </td>
   <td style="text-align:right;"> 42 </td>
   <td style="text-align:right;"> 45.238095 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> charpentier </td>
   <td style="text-align:left;"> Louga </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 3 </td>
   <td style="text-align:right;"> 33.333333 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> charpentier </td>
   <td style="text-align:left;"> Saint Louis </td>
   <td style="text-align:right;"> 2 </td>
   <td style="text-align:right;"> 13 </td>
   <td style="text-align:right;"> 15.384615 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> charpentier </td>
   <td style="text-align:left;"> Thies </td>
   <td style="text-align:right;"> 8 </td>
   <td style="text-align:right;"> 14 </td>
   <td style="text-align:right;"> 57.142857 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> charpentier </td>
   <td style="text-align:left;"> Ziguinchor </td>
   <td style="text-align:right;"> 3 </td>
   <td style="text-align:right;"> 16 </td>
   <td style="text-align:right;"> 18.750000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> electricite </td>
   <td style="text-align:left;"> Dakar </td>
   <td style="text-align:right;"> 14 </td>
   <td style="text-align:right;"> 19 </td>
   <td style="text-align:right;"> 73.684211 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> electricite </td>
   <td style="text-align:left;"> fatick </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 100.000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> electricite </td>
   <td style="text-align:left;"> Fatick </td>
   <td style="text-align:right;"> 14 </td>
   <td style="text-align:right;"> 42 </td>
   <td style="text-align:right;"> 33.333333 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> electricite </td>
   <td style="text-align:left;"> Kaolack </td>
   <td style="text-align:right;"> 2 </td>
   <td style="text-align:right;"> 10 </td>
   <td style="text-align:right;"> 20.000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> electricite </td>
   <td style="text-align:left;"> Louga </td>
   <td style="text-align:right;"> 2 </td>
   <td style="text-align:right;"> 3 </td>
   <td style="text-align:right;"> 66.666667 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> electricite </td>
   <td style="text-align:left;"> Saint Louis </td>
   <td style="text-align:right;"> 11 </td>
   <td style="text-align:right;"> 13 </td>
   <td style="text-align:right;"> 84.615385 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> electricite </td>
   <td style="text-align:left;"> Thies </td>
   <td style="text-align:right;"> 10 </td>
   <td style="text-align:right;"> 14 </td>
   <td style="text-align:right;"> 71.428571 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> electricite </td>
   <td style="text-align:left;"> Ziguinchor </td>
   <td style="text-align:right;"> 3 </td>
   <td style="text-align:right;"> 16 </td>
   <td style="text-align:right;"> 18.750000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> frigo </td>
   <td style="text-align:left;"> Dakar </td>
   <td style="text-align:right;"> 3 </td>
   <td style="text-align:right;"> 19 </td>
   <td style="text-align:right;"> 15.789474 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> frigo </td>
   <td style="text-align:left;"> Fatick </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 42 </td>
   <td style="text-align:right;"> 2.380952 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> frigo </td>
   <td style="text-align:left;"> Saint Louis </td>
   <td style="text-align:right;"> 4 </td>
   <td style="text-align:right;"> 13 </td>
   <td style="text-align:right;"> 30.769231 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> glaciere </td>
   <td style="text-align:left;"> Dakar </td>
   <td style="text-align:right;"> 5 </td>
   <td style="text-align:right;"> 19 </td>
   <td style="text-align:right;"> 26.315790 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> glaciere </td>
   <td style="text-align:left;"> Fatick </td>
   <td style="text-align:right;"> 7 </td>
   <td style="text-align:right;"> 42 </td>
   <td style="text-align:right;"> 16.666667 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> glaciere </td>
   <td style="text-align:left;"> Kaolack </td>
   <td style="text-align:right;"> 2 </td>
   <td style="text-align:right;"> 10 </td>
   <td style="text-align:right;"> 20.000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> glaciere </td>
   <td style="text-align:left;"> Louga </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 3 </td>
   <td style="text-align:right;"> 33.333333 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> glaciere </td>
   <td style="text-align:left;"> Saint Louis </td>
   <td style="text-align:right;"> 7 </td>
   <td style="text-align:right;"> 13 </td>
   <td style="text-align:right;"> 53.846154 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> glaciere </td>
   <td style="text-align:left;"> Thies </td>
   <td style="text-align:right;"> 9 </td>
   <td style="text-align:right;"> 14 </td>
   <td style="text-align:right;"> 64.285714 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> glaciere </td>
   <td style="text-align:left;"> Ziguinchor </td>
   <td style="text-align:right;"> 3 </td>
   <td style="text-align:right;"> 16 </td>
   <td style="text-align:right;"> 18.750000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> halle_poisson </td>
   <td style="text-align:left;"> Dakar </td>
   <td style="text-align:right;"> 5 </td>
   <td style="text-align:right;"> 19 </td>
   <td style="text-align:right;"> 26.315790 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> halle_poisson </td>
   <td style="text-align:left;"> Fatick </td>
   <td style="text-align:right;"> 3 </td>
   <td style="text-align:right;"> 42 </td>
   <td style="text-align:right;"> 7.142857 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> halle_poisson </td>
   <td style="text-align:left;"> Louga </td>
   <td style="text-align:right;"> 2 </td>
   <td style="text-align:right;"> 3 </td>
   <td style="text-align:right;"> 66.666667 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> halle_poisson </td>
   <td style="text-align:left;"> Saint Louis </td>
   <td style="text-align:right;"> 3 </td>
   <td style="text-align:right;"> 13 </td>
   <td style="text-align:right;"> 23.076923 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> halle_poisson </td>
   <td style="text-align:left;"> Thies </td>
   <td style="text-align:right;"> 8 </td>
   <td style="text-align:right;"> 14 </td>
   <td style="text-align:right;"> 57.142857 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> halle_poisson </td>
   <td style="text-align:left;"> Ziguinchor </td>
   <td style="text-align:right;"> 2 </td>
   <td style="text-align:right;"> 16 </td>
   <td style="text-align:right;"> 12.500000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> latrines </td>
   <td style="text-align:left;"> Dakar </td>
   <td style="text-align:right;"> 11 </td>
   <td style="text-align:right;"> 19 </td>
   <td style="text-align:right;"> 57.894737 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> latrines </td>
   <td style="text-align:left;"> Fatick </td>
   <td style="text-align:right;"> 7 </td>
   <td style="text-align:right;"> 42 </td>
   <td style="text-align:right;"> 16.666667 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> latrines </td>
   <td style="text-align:left;"> Louga </td>
   <td style="text-align:right;"> 2 </td>
   <td style="text-align:right;"> 3 </td>
   <td style="text-align:right;"> 66.666667 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> latrines </td>
   <td style="text-align:left;"> Saint Louis </td>
   <td style="text-align:right;"> 2 </td>
   <td style="text-align:right;"> 13 </td>
   <td style="text-align:right;"> 15.384615 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> latrines </td>
   <td style="text-align:left;"> Thies </td>
   <td style="text-align:right;"> 9 </td>
   <td style="text-align:right;"> 14 </td>
   <td style="text-align:right;"> 64.285714 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> latrines </td>
   <td style="text-align:left;"> Ziguinchor </td>
   <td style="text-align:right;"> 3 </td>
   <td style="text-align:right;"> 16 </td>
   <td style="text-align:right;"> 18.750000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> marche </td>
   <td style="text-align:left;"> Dakar </td>
   <td style="text-align:right;"> 7 </td>
   <td style="text-align:right;"> 19 </td>
   <td style="text-align:right;"> 36.842105 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> marche </td>
   <td style="text-align:left;"> Fatick </td>
   <td style="text-align:right;"> 2 </td>
   <td style="text-align:right;"> 42 </td>
   <td style="text-align:right;"> 4.761905 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> marche </td>
   <td style="text-align:left;"> Kaolack </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 10 </td>
   <td style="text-align:right;"> 10.000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> marche </td>
   <td style="text-align:left;"> Saint Louis </td>
   <td style="text-align:right;"> 3 </td>
   <td style="text-align:right;"> 13 </td>
   <td style="text-align:right;"> 23.076923 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> marche </td>
   <td style="text-align:left;"> Thies </td>
   <td style="text-align:right;"> 6 </td>
   <td style="text-align:right;"> 14 </td>
   <td style="text-align:right;"> 42.857143 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> parking </td>
   <td style="text-align:left;"> Dakar </td>
   <td style="text-align:right;"> 5 </td>
   <td style="text-align:right;"> 19 </td>
   <td style="text-align:right;"> 26.315790 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> parking </td>
   <td style="text-align:left;"> Fatick </td>
   <td style="text-align:right;"> 3 </td>
   <td style="text-align:right;"> 42 </td>
   <td style="text-align:right;"> 7.142857 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> parking </td>
   <td style="text-align:left;"> Louga </td>
   <td style="text-align:right;"> 2 </td>
   <td style="text-align:right;"> 3 </td>
   <td style="text-align:right;"> 66.666667 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> parking </td>
   <td style="text-align:left;"> Saint Louis </td>
   <td style="text-align:right;"> 5 </td>
   <td style="text-align:right;"> 13 </td>
   <td style="text-align:right;"> 38.461539 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> parking </td>
   <td style="text-align:left;"> Thies </td>
   <td style="text-align:right;"> 7 </td>
   <td style="text-align:right;"> 14 </td>
   <td style="text-align:right;"> 50.000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> parking </td>
   <td style="text-align:left;"> Ziguinchor </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 16 </td>
   <td style="text-align:right;"> 6.250000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> reparation_meca </td>
   <td style="text-align:left;"> Dakar </td>
   <td style="text-align:right;"> 12 </td>
   <td style="text-align:right;"> 19 </td>
   <td style="text-align:right;"> 63.157895 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> reparation_meca </td>
   <td style="text-align:left;"> Fatick </td>
   <td style="text-align:right;"> 8 </td>
   <td style="text-align:right;"> 42 </td>
   <td style="text-align:right;"> 19.047619 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> reparation_meca </td>
   <td style="text-align:left;"> Louga </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 3 </td>
   <td style="text-align:right;"> 33.333333 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> reparation_meca </td>
   <td style="text-align:left;"> Saint Louis </td>
   <td style="text-align:right;"> 2 </td>
   <td style="text-align:right;"> 13 </td>
   <td style="text-align:right;"> 15.384615 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> reparation_meca </td>
   <td style="text-align:left;"> Thies </td>
   <td style="text-align:right;"> 12 </td>
   <td style="text-align:right;"> 14 </td>
   <td style="text-align:right;"> 85.714286 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> reparation_meca </td>
   <td style="text-align:left;"> Ziguinchor </td>
   <td style="text-align:right;"> 4 </td>
   <td style="text-align:right;"> 16 </td>
   <td style="text-align:right;"> 25.000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> robinet </td>
   <td style="text-align:left;"> Dakar </td>
   <td style="text-align:right;"> 8 </td>
   <td style="text-align:right;"> 19 </td>
   <td style="text-align:right;"> 42.105263 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> robinet </td>
   <td style="text-align:left;"> Fatick </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 42 </td>
   <td style="text-align:right;"> 2.380952 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> robinet </td>
   <td style="text-align:left;"> Louga </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 3 </td>
   <td style="text-align:right;"> 33.333333 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> robinet </td>
   <td style="text-align:left;"> Thies </td>
   <td style="text-align:right;"> 9 </td>
   <td style="text-align:right;"> 14 </td>
   <td style="text-align:right;"> 64.285714 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> robinet </td>
   <td style="text-align:left;"> Ziguinchor </td>
   <td style="text-align:right;"> 4 </td>
   <td style="text-align:right;"> 16 </td>
   <td style="text-align:right;"> 25.000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> sans_amenagement </td>
   <td style="text-align:left;"> Dakar </td>
   <td style="text-align:right;"> 14 </td>
   <td style="text-align:right;"> 19 </td>
   <td style="text-align:right;"> 73.684211 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> sans_amenagement </td>
   <td style="text-align:left;"> Fatick </td>
   <td style="text-align:right;"> 36 </td>
   <td style="text-align:right;"> 42 </td>
   <td style="text-align:right;"> 85.714286 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> sans_amenagement </td>
   <td style="text-align:left;"> Kaolack </td>
   <td style="text-align:right;"> 9 </td>
   <td style="text-align:right;"> 10 </td>
   <td style="text-align:right;"> 90.000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> sans_amenagement </td>
   <td style="text-align:left;"> Louga </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 3 </td>
   <td style="text-align:right;"> 33.333333 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> sans_amenagement </td>
   <td style="text-align:left;"> Saint Louis </td>
   <td style="text-align:right;"> 9 </td>
   <td style="text-align:right;"> 13 </td>
   <td style="text-align:right;"> 69.230769 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> sans_amenagement </td>
   <td style="text-align:left;"> Thies </td>
   <td style="text-align:right;"> 6 </td>
   <td style="text-align:right;"> 14 </td>
   <td style="text-align:right;"> 42.857143 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> sans_amenagement </td>
   <td style="text-align:left;"> ziguinchor </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 100.000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> sans_amenagement </td>
   <td style="text-align:left;"> Ziguinchor </td>
   <td style="text-align:right;"> 13 </td>
   <td style="text-align:right;"> 16 </td>
   <td style="text-align:right;"> 81.250000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> site_transfo_amenage </td>
   <td style="text-align:left;"> Dakar </td>
   <td style="text-align:right;"> 7 </td>
   <td style="text-align:right;"> 19 </td>
   <td style="text-align:right;"> 36.842105 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> site_transfo_amenage </td>
   <td style="text-align:left;"> fatick </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 100.000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> site_transfo_amenage </td>
   <td style="text-align:left;"> Fatick </td>
   <td style="text-align:right;"> 9 </td>
   <td style="text-align:right;"> 42 </td>
   <td style="text-align:right;"> 21.428571 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> site_transfo_amenage </td>
   <td style="text-align:left;"> Kaolack </td>
   <td style="text-align:right;"> 2 </td>
   <td style="text-align:right;"> 10 </td>
   <td style="text-align:right;"> 20.000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> site_transfo_amenage </td>
   <td style="text-align:left;"> Louga </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 3 </td>
   <td style="text-align:right;"> 33.333333 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> site_transfo_amenage </td>
   <td style="text-align:left;"> Saint Louis </td>
   <td style="text-align:right;"> 2 </td>
   <td style="text-align:right;"> 13 </td>
   <td style="text-align:right;"> 15.384615 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> site_transfo_amenage </td>
   <td style="text-align:left;"> Thies </td>
   <td style="text-align:right;"> 5 </td>
   <td style="text-align:right;"> 14 </td>
   <td style="text-align:right;"> 35.714286 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> site_transfo_amenage </td>
   <td style="text-align:left;"> Ziguinchor </td>
   <td style="text-align:right;"> 3 </td>
   <td style="text-align:right;"> 16 </td>
   <td style="text-align:right;"> 18.750000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> telphone </td>
   <td style="text-align:left;"> Dakar </td>
   <td style="text-align:right;"> 17 </td>
   <td style="text-align:right;"> 19 </td>
   <td style="text-align:right;"> 89.473684 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> telphone </td>
   <td style="text-align:left;"> fatick </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 100.000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> telphone </td>
   <td style="text-align:left;"> Fatick </td>
   <td style="text-align:right;"> 26 </td>
   <td style="text-align:right;"> 42 </td>
   <td style="text-align:right;"> 61.904762 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> telphone </td>
   <td style="text-align:left;"> Kaolack </td>
   <td style="text-align:right;"> 10 </td>
   <td style="text-align:right;"> 10 </td>
   <td style="text-align:right;"> 100.000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> telphone </td>
   <td style="text-align:left;"> Louga </td>
   <td style="text-align:right;"> 3 </td>
   <td style="text-align:right;"> 3 </td>
   <td style="text-align:right;"> 100.000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> telphone </td>
   <td style="text-align:left;"> Saint Louis </td>
   <td style="text-align:right;"> 12 </td>
   <td style="text-align:right;"> 13 </td>
   <td style="text-align:right;"> 92.307692 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> telphone </td>
   <td style="text-align:left;"> Thies </td>
   <td style="text-align:right;"> 14 </td>
   <td style="text-align:right;"> 14 </td>
   <td style="text-align:right;"> 100.000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> telphone </td>
   <td style="text-align:left;"> Ziguinchor </td>
   <td style="text-align:right;"> 14 </td>
   <td style="text-align:right;"> 16 </td>
   <td style="text-align:right;"> 87.500000 </td>
  </tr>
</tbody>
</table>

```r
ggplot(commodites.reg)+
  geom_bar(stat='identity',aes(x=Commodites,y=pour.site_debarq))+
  facet_wrap(~niveau_adm1)+
   theme(axis.text.x = element_text(angle = 90, vjust = 0.5, hjust=1))
```

<img src="/tmp/RtmpItTkgh/preview-2658e22e89d0f4.dir/Rapports_base2_files/figure-html/unnamed-chunk-11-1.png" width="672" />

Ceux qui n'ont pas les commodites

```r
total.reg<- liste.sites.init %>% group_by(niveau_adm1) %>%
  dplyr::summarise(total.reg=n())


liste.sites.init[,c(1,13,seq(22,38))]  %>%
  pivot_longer( cols = names(liste.sites)[c(seq(22,38))], names_to = "Commodites") %>%
  filter(value=='Non') %>%
  group_by(Commodites,niveau_adm1) %>%
  summarize(nb.sites=n()) %>% inner_join(total.reg) %>%
   dplyr::mutate(pour.site_debarq=100*nb.sites/total.reg)->commodites.reg
```

```
## `summarise()` has grouped output by 'Commodites'. You can override using the
## `.groups` argument.
## Joining, by = "niveau_adm1"
```

```r
commodites.reg %>%
  kbl() %>% kable_styling()
```

<table class="table" style="margin-left: auto; margin-right: auto;">
 <thead>
  <tr>
   <th style="text-align:left;"> Commodites </th>
   <th style="text-align:left;"> niveau_adm1 </th>
   <th style="text-align:right;"> nb.sites </th>
   <th style="text-align:right;"> total.reg </th>
   <th style="text-align:right;"> pour.site_debarq </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:left;"> amenage </td>
   <td style="text-align:left;"> Dakar </td>
   <td style="text-align:right;"> 13 </td>
   <td style="text-align:right;"> 19 </td>
   <td style="text-align:right;"> 68.421053 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> amenage </td>
   <td style="text-align:left;"> Fatick </td>
   <td style="text-align:right;"> 39 </td>
   <td style="text-align:right;"> 42 </td>
   <td style="text-align:right;"> 92.857143 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> amenage </td>
   <td style="text-align:left;"> Kaolack </td>
   <td style="text-align:right;"> 10 </td>
   <td style="text-align:right;"> 10 </td>
   <td style="text-align:right;"> 100.000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> amenage </td>
   <td style="text-align:left;"> Louga </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 3 </td>
   <td style="text-align:right;"> 33.333333 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> amenage </td>
   <td style="text-align:left;"> Saint Louis </td>
   <td style="text-align:right;"> 9 </td>
   <td style="text-align:right;"> 13 </td>
   <td style="text-align:right;"> 69.230769 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> amenage </td>
   <td style="text-align:left;"> Thies </td>
   <td style="text-align:right;"> 6 </td>
   <td style="text-align:right;"> 14 </td>
   <td style="text-align:right;"> 42.857143 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> amenage </td>
   <td style="text-align:left;"> ziguinchor </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 100.000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> amenage </td>
   <td style="text-align:left;"> Ziguinchor </td>
   <td style="text-align:right;"> 12 </td>
   <td style="text-align:right;"> 16 </td>
   <td style="text-align:right;"> 75.000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> app_carburant </td>
   <td style="text-align:left;"> Dakar </td>
   <td style="text-align:right;"> 8 </td>
   <td style="text-align:right;"> 19 </td>
   <td style="text-align:right;"> 42.105263 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> app_carburant </td>
   <td style="text-align:left;"> fatick </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 100.000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> app_carburant </td>
   <td style="text-align:left;"> Fatick </td>
   <td style="text-align:right;"> 32 </td>
   <td style="text-align:right;"> 42 </td>
   <td style="text-align:right;"> 76.190476 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> app_carburant </td>
   <td style="text-align:left;"> Kaolack </td>
   <td style="text-align:right;"> 9 </td>
   <td style="text-align:right;"> 10 </td>
   <td style="text-align:right;"> 90.000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> app_carburant </td>
   <td style="text-align:left;"> Saint Louis </td>
   <td style="text-align:right;"> 7 </td>
   <td style="text-align:right;"> 13 </td>
   <td style="text-align:right;"> 53.846154 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> app_carburant </td>
   <td style="text-align:left;"> Thies </td>
   <td style="text-align:right;"> 3 </td>
   <td style="text-align:right;"> 14 </td>
   <td style="text-align:right;"> 21.428571 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> app_carburant </td>
   <td style="text-align:left;"> Ziguinchor </td>
   <td style="text-align:right;"> 11 </td>
   <td style="text-align:right;"> 16 </td>
   <td style="text-align:right;"> 68.750000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> app_glace </td>
   <td style="text-align:left;"> Dakar </td>
   <td style="text-align:right;"> 14 </td>
   <td style="text-align:right;"> 19 </td>
   <td style="text-align:right;"> 73.684211 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> app_glace </td>
   <td style="text-align:left;"> fatick </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 100.000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> app_glace </td>
   <td style="text-align:left;"> Fatick </td>
   <td style="text-align:right;"> 39 </td>
   <td style="text-align:right;"> 42 </td>
   <td style="text-align:right;"> 92.857143 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> app_glace </td>
   <td style="text-align:left;"> Kaolack </td>
   <td style="text-align:right;"> 9 </td>
   <td style="text-align:right;"> 10 </td>
   <td style="text-align:right;"> 90.000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> app_glace </td>
   <td style="text-align:left;"> Louga </td>
   <td style="text-align:right;"> 3 </td>
   <td style="text-align:right;"> 3 </td>
   <td style="text-align:right;"> 100.000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> app_glace </td>
   <td style="text-align:left;"> Saint Louis </td>
   <td style="text-align:right;"> 8 </td>
   <td style="text-align:right;"> 13 </td>
   <td style="text-align:right;"> 61.538461 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> app_glace </td>
   <td style="text-align:left;"> Thies </td>
   <td style="text-align:right;"> 4 </td>
   <td style="text-align:right;"> 14 </td>
   <td style="text-align:right;"> 28.571429 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> app_glace </td>
   <td style="text-align:left;"> Ziguinchor </td>
   <td style="text-align:right;"> 12 </td>
   <td style="text-align:right;"> 16 </td>
   <td style="text-align:right;"> 75.000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> boutique </td>
   <td style="text-align:left;"> Dakar </td>
   <td style="text-align:right;"> 5 </td>
   <td style="text-align:right;"> 19 </td>
   <td style="text-align:right;"> 26.315790 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> boutique </td>
   <td style="text-align:left;"> fatick </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 100.000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> boutique </td>
   <td style="text-align:left;"> Fatick </td>
   <td style="text-align:right;"> 33 </td>
   <td style="text-align:right;"> 42 </td>
   <td style="text-align:right;"> 78.571429 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> boutique </td>
   <td style="text-align:left;"> Kaolack </td>
   <td style="text-align:right;"> 10 </td>
   <td style="text-align:right;"> 10 </td>
   <td style="text-align:right;"> 100.000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> boutique </td>
   <td style="text-align:left;"> Louga </td>
   <td style="text-align:right;"> 2 </td>
   <td style="text-align:right;"> 3 </td>
   <td style="text-align:right;"> 66.666667 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> boutique </td>
   <td style="text-align:left;"> Saint Louis </td>
   <td style="text-align:right;"> 3 </td>
   <td style="text-align:right;"> 13 </td>
   <td style="text-align:right;"> 23.076923 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> boutique </td>
   <td style="text-align:left;"> Thies </td>
   <td style="text-align:right;"> 4 </td>
   <td style="text-align:right;"> 14 </td>
   <td style="text-align:right;"> 28.571429 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> boutique </td>
   <td style="text-align:left;"> ziguinchor </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 100.000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> boutique </td>
   <td style="text-align:left;"> Ziguinchor </td>
   <td style="text-align:right;"> 11 </td>
   <td style="text-align:right;"> 16 </td>
   <td style="text-align:right;"> 68.750000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> charpentier </td>
   <td style="text-align:left;"> Dakar </td>
   <td style="text-align:right;"> 4 </td>
   <td style="text-align:right;"> 19 </td>
   <td style="text-align:right;"> 21.052632 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> charpentier </td>
   <td style="text-align:left;"> Fatick </td>
   <td style="text-align:right;"> 19 </td>
   <td style="text-align:right;"> 42 </td>
   <td style="text-align:right;"> 45.238095 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> charpentier </td>
   <td style="text-align:left;"> Kaolack </td>
   <td style="text-align:right;"> 10 </td>
   <td style="text-align:right;"> 10 </td>
   <td style="text-align:right;"> 100.000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> charpentier </td>
   <td style="text-align:left;"> Louga </td>
   <td style="text-align:right;"> 2 </td>
   <td style="text-align:right;"> 3 </td>
   <td style="text-align:right;"> 66.666667 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> charpentier </td>
   <td style="text-align:left;"> Saint Louis </td>
   <td style="text-align:right;"> 9 </td>
   <td style="text-align:right;"> 13 </td>
   <td style="text-align:right;"> 69.230769 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> charpentier </td>
   <td style="text-align:left;"> Thies </td>
   <td style="text-align:right;"> 6 </td>
   <td style="text-align:right;"> 14 </td>
   <td style="text-align:right;"> 42.857143 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> charpentier </td>
   <td style="text-align:left;"> ziguinchor </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 100.000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> charpentier </td>
   <td style="text-align:left;"> Ziguinchor </td>
   <td style="text-align:right;"> 12 </td>
   <td style="text-align:right;"> 16 </td>
   <td style="text-align:right;"> 75.000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> electricite </td>
   <td style="text-align:left;"> Dakar </td>
   <td style="text-align:right;"> 4 </td>
   <td style="text-align:right;"> 19 </td>
   <td style="text-align:right;"> 21.052632 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> electricite </td>
   <td style="text-align:left;"> Fatick </td>
   <td style="text-align:right;"> 24 </td>
   <td style="text-align:right;"> 42 </td>
   <td style="text-align:right;"> 57.142857 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> electricite </td>
   <td style="text-align:left;"> Kaolack </td>
   <td style="text-align:right;"> 8 </td>
   <td style="text-align:right;"> 10 </td>
   <td style="text-align:right;"> 80.000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> electricite </td>
   <td style="text-align:left;"> Louga </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 3 </td>
   <td style="text-align:right;"> 33.333333 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> electricite </td>
   <td style="text-align:left;"> Saint Louis </td>
   <td style="text-align:right;"> 2 </td>
   <td style="text-align:right;"> 13 </td>
   <td style="text-align:right;"> 15.384615 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> electricite </td>
   <td style="text-align:left;"> Thies </td>
   <td style="text-align:right;"> 4 </td>
   <td style="text-align:right;"> 14 </td>
   <td style="text-align:right;"> 28.571429 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> electricite </td>
   <td style="text-align:left;"> ziguinchor </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 100.000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> electricite </td>
   <td style="text-align:left;"> Ziguinchor </td>
   <td style="text-align:right;"> 12 </td>
   <td style="text-align:right;"> 16 </td>
   <td style="text-align:right;"> 75.000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> frigo </td>
   <td style="text-align:left;"> Dakar </td>
   <td style="text-align:right;"> 15 </td>
   <td style="text-align:right;"> 19 </td>
   <td style="text-align:right;"> 78.947368 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> frigo </td>
   <td style="text-align:left;"> fatick </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 100.000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> frigo </td>
   <td style="text-align:left;"> Fatick </td>
   <td style="text-align:right;"> 39 </td>
   <td style="text-align:right;"> 42 </td>
   <td style="text-align:right;"> 92.857143 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> frigo </td>
   <td style="text-align:left;"> Kaolack </td>
   <td style="text-align:right;"> 10 </td>
   <td style="text-align:right;"> 10 </td>
   <td style="text-align:right;"> 100.000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> frigo </td>
   <td style="text-align:left;"> Louga </td>
   <td style="text-align:right;"> 3 </td>
   <td style="text-align:right;"> 3 </td>
   <td style="text-align:right;"> 100.000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> frigo </td>
   <td style="text-align:left;"> Saint Louis </td>
   <td style="text-align:right;"> 8 </td>
   <td style="text-align:right;"> 13 </td>
   <td style="text-align:right;"> 61.538461 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> frigo </td>
   <td style="text-align:left;"> Thies </td>
   <td style="text-align:right;"> 12 </td>
   <td style="text-align:right;"> 14 </td>
   <td style="text-align:right;"> 85.714286 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> frigo </td>
   <td style="text-align:left;"> ziguinchor </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 100.000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> frigo </td>
   <td style="text-align:left;"> Ziguinchor </td>
   <td style="text-align:right;"> 15 </td>
   <td style="text-align:right;"> 16 </td>
   <td style="text-align:right;"> 93.750000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> glaciere </td>
   <td style="text-align:left;"> Dakar </td>
   <td style="text-align:right;"> 12 </td>
   <td style="text-align:right;"> 19 </td>
   <td style="text-align:right;"> 63.157895 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> glaciere </td>
   <td style="text-align:left;"> fatick </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 100.000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> glaciere </td>
   <td style="text-align:left;"> Fatick </td>
   <td style="text-align:right;"> 32 </td>
   <td style="text-align:right;"> 42 </td>
   <td style="text-align:right;"> 76.190476 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> glaciere </td>
   <td style="text-align:left;"> Kaolack </td>
   <td style="text-align:right;"> 8 </td>
   <td style="text-align:right;"> 10 </td>
   <td style="text-align:right;"> 80.000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> glaciere </td>
   <td style="text-align:left;"> Louga </td>
   <td style="text-align:right;"> 2 </td>
   <td style="text-align:right;"> 3 </td>
   <td style="text-align:right;"> 66.666667 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> glaciere </td>
   <td style="text-align:left;"> Saint Louis </td>
   <td style="text-align:right;"> 4 </td>
   <td style="text-align:right;"> 13 </td>
   <td style="text-align:right;"> 30.769231 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> glaciere </td>
   <td style="text-align:left;"> Thies </td>
   <td style="text-align:right;"> 5 </td>
   <td style="text-align:right;"> 14 </td>
   <td style="text-align:right;"> 35.714286 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> glaciere </td>
   <td style="text-align:left;"> Ziguinchor </td>
   <td style="text-align:right;"> 11 </td>
   <td style="text-align:right;"> 16 </td>
   <td style="text-align:right;"> 68.750000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> halle_poisson </td>
   <td style="text-align:left;"> Dakar </td>
   <td style="text-align:right;"> 14 </td>
   <td style="text-align:right;"> 19 </td>
   <td style="text-align:right;"> 73.684211 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> halle_poisson </td>
   <td style="text-align:left;"> fatick </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 100.000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> halle_poisson </td>
   <td style="text-align:left;"> Fatick </td>
   <td style="text-align:right;"> 38 </td>
   <td style="text-align:right;"> 42 </td>
   <td style="text-align:right;"> 90.476191 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> halle_poisson </td>
   <td style="text-align:left;"> Kaolack </td>
   <td style="text-align:right;"> 10 </td>
   <td style="text-align:right;"> 10 </td>
   <td style="text-align:right;"> 100.000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> halle_poisson </td>
   <td style="text-align:left;"> Louga </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 3 </td>
   <td style="text-align:right;"> 33.333333 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> halle_poisson </td>
   <td style="text-align:left;"> Saint Louis </td>
   <td style="text-align:right;"> 10 </td>
   <td style="text-align:right;"> 13 </td>
   <td style="text-align:right;"> 76.923077 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> halle_poisson </td>
   <td style="text-align:left;"> Thies </td>
   <td style="text-align:right;"> 6 </td>
   <td style="text-align:right;"> 14 </td>
   <td style="text-align:right;"> 42.857143 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> halle_poisson </td>
   <td style="text-align:left;"> ziguinchor </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 100.000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> halle_poisson </td>
   <td style="text-align:left;"> Ziguinchor </td>
   <td style="text-align:right;"> 13 </td>
   <td style="text-align:right;"> 16 </td>
   <td style="text-align:right;"> 81.250000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> latrines </td>
   <td style="text-align:left;"> Dakar </td>
   <td style="text-align:right;"> 8 </td>
   <td style="text-align:right;"> 19 </td>
   <td style="text-align:right;"> 42.105263 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> latrines </td>
   <td style="text-align:left;"> fatick </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 100.000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> latrines </td>
   <td style="text-align:left;"> Fatick </td>
   <td style="text-align:right;"> 34 </td>
   <td style="text-align:right;"> 42 </td>
   <td style="text-align:right;"> 80.952381 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> latrines </td>
   <td style="text-align:left;"> Kaolack </td>
   <td style="text-align:right;"> 10 </td>
   <td style="text-align:right;"> 10 </td>
   <td style="text-align:right;"> 100.000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> latrines </td>
   <td style="text-align:left;"> Louga </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 3 </td>
   <td style="text-align:right;"> 33.333333 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> latrines </td>
   <td style="text-align:left;"> Saint Louis </td>
   <td style="text-align:right;"> 10 </td>
   <td style="text-align:right;"> 13 </td>
   <td style="text-align:right;"> 76.923077 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> latrines </td>
   <td style="text-align:left;"> Thies </td>
   <td style="text-align:right;"> 5 </td>
   <td style="text-align:right;"> 14 </td>
   <td style="text-align:right;"> 35.714286 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> latrines </td>
   <td style="text-align:left;"> ziguinchor </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 100.000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> latrines </td>
   <td style="text-align:left;"> Ziguinchor </td>
   <td style="text-align:right;"> 12 </td>
   <td style="text-align:right;"> 16 </td>
   <td style="text-align:right;"> 75.000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> marche </td>
   <td style="text-align:left;"> Dakar </td>
   <td style="text-align:right;"> 12 </td>
   <td style="text-align:right;"> 19 </td>
   <td style="text-align:right;"> 63.157895 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> marche </td>
   <td style="text-align:left;"> fatick </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 100.000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> marche </td>
   <td style="text-align:left;"> Fatick </td>
   <td style="text-align:right;"> 38 </td>
   <td style="text-align:right;"> 42 </td>
   <td style="text-align:right;"> 90.476191 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> marche </td>
   <td style="text-align:left;"> Kaolack </td>
   <td style="text-align:right;"> 9 </td>
   <td style="text-align:right;"> 10 </td>
   <td style="text-align:right;"> 90.000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> marche </td>
   <td style="text-align:left;"> Louga </td>
   <td style="text-align:right;"> 3 </td>
   <td style="text-align:right;"> 3 </td>
   <td style="text-align:right;"> 100.000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> marche </td>
   <td style="text-align:left;"> Saint Louis </td>
   <td style="text-align:right;"> 10 </td>
   <td style="text-align:right;"> 13 </td>
   <td style="text-align:right;"> 76.923077 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> marche </td>
   <td style="text-align:left;"> Thies </td>
   <td style="text-align:right;"> 8 </td>
   <td style="text-align:right;"> 14 </td>
   <td style="text-align:right;"> 57.142857 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> marche </td>
   <td style="text-align:left;"> ziguinchor </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 100.000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> marche </td>
   <td style="text-align:left;"> Ziguinchor </td>
   <td style="text-align:right;"> 15 </td>
   <td style="text-align:right;"> 16 </td>
   <td style="text-align:right;"> 93.750000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> parking </td>
   <td style="text-align:left;"> Dakar </td>
   <td style="text-align:right;"> 14 </td>
   <td style="text-align:right;"> 19 </td>
   <td style="text-align:right;"> 73.684211 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> parking </td>
   <td style="text-align:left;"> fatick </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 100.000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> parking </td>
   <td style="text-align:left;"> Fatick </td>
   <td style="text-align:right;"> 38 </td>
   <td style="text-align:right;"> 42 </td>
   <td style="text-align:right;"> 90.476191 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> parking </td>
   <td style="text-align:left;"> Kaolack </td>
   <td style="text-align:right;"> 10 </td>
   <td style="text-align:right;"> 10 </td>
   <td style="text-align:right;"> 100.000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> parking </td>
   <td style="text-align:left;"> Louga </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 3 </td>
   <td style="text-align:right;"> 33.333333 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> parking </td>
   <td style="text-align:left;"> Saint Louis </td>
   <td style="text-align:right;"> 8 </td>
   <td style="text-align:right;"> 13 </td>
   <td style="text-align:right;"> 61.538461 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> parking </td>
   <td style="text-align:left;"> Thies </td>
   <td style="text-align:right;"> 7 </td>
   <td style="text-align:right;"> 14 </td>
   <td style="text-align:right;"> 50.000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> parking </td>
   <td style="text-align:left;"> ziguinchor </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 100.000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> parking </td>
   <td style="text-align:left;"> Ziguinchor </td>
   <td style="text-align:right;"> 14 </td>
   <td style="text-align:right;"> 16 </td>
   <td style="text-align:right;"> 87.500000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> reparation_meca </td>
   <td style="text-align:left;"> Dakar </td>
   <td style="text-align:right;"> 6 </td>
   <td style="text-align:right;"> 19 </td>
   <td style="text-align:right;"> 31.578947 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> reparation_meca </td>
   <td style="text-align:left;"> fatick </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 100.000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> reparation_meca </td>
   <td style="text-align:left;"> Fatick </td>
   <td style="text-align:right;"> 31 </td>
   <td style="text-align:right;"> 42 </td>
   <td style="text-align:right;"> 73.809524 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> reparation_meca </td>
   <td style="text-align:left;"> Kaolack </td>
   <td style="text-align:right;"> 10 </td>
   <td style="text-align:right;"> 10 </td>
   <td style="text-align:right;"> 100.000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> reparation_meca </td>
   <td style="text-align:left;"> Louga </td>
   <td style="text-align:right;"> 2 </td>
   <td style="text-align:right;"> 3 </td>
   <td style="text-align:right;"> 66.666667 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> reparation_meca </td>
   <td style="text-align:left;"> Saint Louis </td>
   <td style="text-align:right;"> 9 </td>
   <td style="text-align:right;"> 13 </td>
   <td style="text-align:right;"> 69.230769 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> reparation_meca </td>
   <td style="text-align:left;"> Thies </td>
   <td style="text-align:right;"> 2 </td>
   <td style="text-align:right;"> 14 </td>
   <td style="text-align:right;"> 14.285714 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> reparation_meca </td>
   <td style="text-align:left;"> Ziguinchor </td>
   <td style="text-align:right;"> 11 </td>
   <td style="text-align:right;"> 16 </td>
   <td style="text-align:right;"> 68.750000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> robinet </td>
   <td style="text-align:left;"> Dakar </td>
   <td style="text-align:right;"> 11 </td>
   <td style="text-align:right;"> 19 </td>
   <td style="text-align:right;"> 57.894737 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> robinet </td>
   <td style="text-align:left;"> Fatick </td>
   <td style="text-align:right;"> 39 </td>
   <td style="text-align:right;"> 42 </td>
   <td style="text-align:right;"> 92.857143 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> robinet </td>
   <td style="text-align:left;"> Kaolack </td>
   <td style="text-align:right;"> 10 </td>
   <td style="text-align:right;"> 10 </td>
   <td style="text-align:right;"> 100.000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> robinet </td>
   <td style="text-align:left;"> Louga </td>
   <td style="text-align:right;"> 2 </td>
   <td style="text-align:right;"> 3 </td>
   <td style="text-align:right;"> 66.666667 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> robinet </td>
   <td style="text-align:left;"> Saint Louis </td>
   <td style="text-align:right;"> 13 </td>
   <td style="text-align:right;"> 13 </td>
   <td style="text-align:right;"> 100.000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> robinet </td>
   <td style="text-align:left;"> Thies </td>
   <td style="text-align:right;"> 5 </td>
   <td style="text-align:right;"> 14 </td>
   <td style="text-align:right;"> 35.714286 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> robinet </td>
   <td style="text-align:left;"> ziguinchor </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 100.000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> robinet </td>
   <td style="text-align:left;"> Ziguinchor </td>
   <td style="text-align:right;"> 11 </td>
   <td style="text-align:right;"> 16 </td>
   <td style="text-align:right;"> 68.750000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> sans_amenagement </td>
   <td style="text-align:left;"> Dakar </td>
   <td style="text-align:right;"> 5 </td>
   <td style="text-align:right;"> 19 </td>
   <td style="text-align:right;"> 26.315790 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> sans_amenagement </td>
   <td style="text-align:left;"> fatick </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 100.000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> sans_amenagement </td>
   <td style="text-align:left;"> Fatick </td>
   <td style="text-align:right;"> 5 </td>
   <td style="text-align:right;"> 42 </td>
   <td style="text-align:right;"> 11.904762 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> sans_amenagement </td>
   <td style="text-align:left;"> Kaolack </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 10 </td>
   <td style="text-align:right;"> 10.000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> sans_amenagement </td>
   <td style="text-align:left;"> Louga </td>
   <td style="text-align:right;"> 2 </td>
   <td style="text-align:right;"> 3 </td>
   <td style="text-align:right;"> 66.666667 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> sans_amenagement </td>
   <td style="text-align:left;"> Saint Louis </td>
   <td style="text-align:right;"> 4 </td>
   <td style="text-align:right;"> 13 </td>
   <td style="text-align:right;"> 30.769231 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> sans_amenagement </td>
   <td style="text-align:left;"> Thies </td>
   <td style="text-align:right;"> 8 </td>
   <td style="text-align:right;"> 14 </td>
   <td style="text-align:right;"> 57.142857 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> sans_amenagement </td>
   <td style="text-align:left;"> Ziguinchor </td>
   <td style="text-align:right;"> 2 </td>
   <td style="text-align:right;"> 16 </td>
   <td style="text-align:right;"> 12.500000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> site_transfo_amenage </td>
   <td style="text-align:left;"> Dakar </td>
   <td style="text-align:right;"> 12 </td>
   <td style="text-align:right;"> 19 </td>
   <td style="text-align:right;"> 63.157895 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> site_transfo_amenage </td>
   <td style="text-align:left;"> Fatick </td>
   <td style="text-align:right;"> 31 </td>
   <td style="text-align:right;"> 42 </td>
   <td style="text-align:right;"> 73.809524 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> site_transfo_amenage </td>
   <td style="text-align:left;"> Kaolack </td>
   <td style="text-align:right;"> 8 </td>
   <td style="text-align:right;"> 10 </td>
   <td style="text-align:right;"> 80.000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> site_transfo_amenage </td>
   <td style="text-align:left;"> Louga </td>
   <td style="text-align:right;"> 2 </td>
   <td style="text-align:right;"> 3 </td>
   <td style="text-align:right;"> 66.666667 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> site_transfo_amenage </td>
   <td style="text-align:left;"> Saint Louis </td>
   <td style="text-align:right;"> 11 </td>
   <td style="text-align:right;"> 13 </td>
   <td style="text-align:right;"> 84.615385 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> site_transfo_amenage </td>
   <td style="text-align:left;"> Thies </td>
   <td style="text-align:right;"> 8 </td>
   <td style="text-align:right;"> 14 </td>
   <td style="text-align:right;"> 57.142857 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> site_transfo_amenage </td>
   <td style="text-align:left;"> ziguinchor </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 100.000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> site_transfo_amenage </td>
   <td style="text-align:left;"> Ziguinchor </td>
   <td style="text-align:right;"> 12 </td>
   <td style="text-align:right;"> 16 </td>
   <td style="text-align:right;"> 75.000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> telphone </td>
   <td style="text-align:left;"> Dakar </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 19 </td>
   <td style="text-align:right;"> 5.263158 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> telphone </td>
   <td style="text-align:left;"> Fatick </td>
   <td style="text-align:right;"> 12 </td>
   <td style="text-align:right;"> 42 </td>
   <td style="text-align:right;"> 28.571429 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> telphone </td>
   <td style="text-align:left;"> ziguinchor </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 100.000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> telphone </td>
   <td style="text-align:left;"> Ziguinchor </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 16 </td>
   <td style="text-align:right;"> 6.250000 </td>
  </tr>
</tbody>
</table>

```r
ggplot(commodites.reg)+
  geom_bar(stat='identity',aes(x=Commodites,y=pour.site_debarq))+
  facet_wrap(~niveau_adm1)+
   theme(axis.text.x = element_text(angle = 90, vjust = 0.5, hjust=1))
```

<img src="/tmp/RtmpItTkgh/preview-2658e22e89d0f4.dir/Rapports_base2_files/figure-html/unnamed-chunk-12-1.png" width="672" />

## Coûts des activités de pêche

### Cout variable moyen d'activité par U.P., au niveau mensuel puis annuel (par catégorie d'U.P.)

Multiplier le coût par sortie par le nombre de sorties par mois. Pour passer au niveau annuel, multiplié par le nombre de mois effectué avec la technique. Le cas échéant compléter pour l'U.P. par le nombre de mois fait avec l'autre technique.


```r
test.eco=nrow(donnees$eco_unite)!=0
```



```r
listes_site_eco.init<- donnees$eco_unite %>% mutate(pays=toupper(pays)) %>% mutate(libel_site=(nom_site)) %>% inner_join(donnees$ref_sites,by=c('pays','libel_site'))
```



```r
listes_site_eco.init %>% 
select(c('region_niv1','technique1','nb_sortie_mois_tech1','cout_carburant_tech1','cout_glace_tech1','cout_appat_tech1','cout_nourriture_tech1','val_basse_tech1','val_haute_tech1','val_moyenne_tech1')) %>% 
  mutate(cout.total.mois=nb_sortie_mois_tech1
*(coalesce(cout_carburant_tech1,0)+coalesce(cout_glace_tech1,0)+
    coalesce(cout_appat_tech1,0)+coalesce(cout_nourriture_tech1,0)),
recette.total.mois=nb_sortie_mois_tech1*val_moyenne_tech1) %>% 
  dplyr::group_by(technique1) %>% 
  dplyr::summarise(cout.moyen=mean(cout.total.mois,na.tm=TRUE),recette.moy=mean(recette.total.mois,na.rm=TRUE))->couts.moyens

couts.moyens %>% kbl() %>% 
  kable_styling()
```

<table class="table" style="margin-left: auto; margin-right: auto;">
 <thead>
  <tr>
   <th style="text-align:left;"> technique1 </th>
   <th style="text-align:right;"> cout.moyen </th>
   <th style="text-align:right;"> recette.moy </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:left;"> AU </td>
   <td style="text-align:right;"> 216928.6 </td>
   <td style="text-align:right;"> 980642.9 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> EP </td>
   <td style="text-align:right;"> 79687.5 </td>
   <td style="text-align:right;"> 419968.8 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> FMCF </td>
   <td style="text-align:right;"> 782253.8 </td>
   <td style="text-align:right;"> 3144388.1 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> FMCS </td>
   <td style="text-align:right;"> 196200.0 </td>
   <td style="text-align:right;"> 828000.0 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> FMD </td>
   <td style="text-align:right;"> 857798.8 </td>
   <td style="text-align:right;"> 2103024.4 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> FMDS </td>
   <td style="text-align:right;"> 455000.0 </td>
   <td style="text-align:right;"> 1820000.0 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> FME </td>
   <td style="text-align:right;"> 642623.6 </td>
   <td style="text-align:right;"> 3493015.2 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> FS </td>
   <td style="text-align:right;"> 1190000.0 </td>
   <td style="text-align:right;"> 2288000.0 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> LI </td>
   <td style="text-align:right;"> 1098955.3 </td>
   <td style="text-align:right;"> 3364619.7 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> PA </td>
   <td style="text-align:right;"> 2360638.9 </td>
   <td style="text-align:right;"> 7654541.7 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> PIE </td>
   <td style="text-align:right;"> 908230.8 </td>
   <td style="text-align:right;"> 1763307.7 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> SP </td>
   <td style="text-align:right;"> NA </td>
   <td style="text-align:right;"> 4187554.2 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> ST </td>
   <td style="text-align:right;"> 5944284.0 </td>
   <td style="text-align:right;"> 30153250.0 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:right;"> 100000.0 </td>
   <td style="text-align:right;"> 731272.7 </td>
  </tr>
</tbody>
</table>

```r
 couts.moyens %>% pivot_longer(!technique1,names_to = 'Indic') %>% 

   ggplot()+ geom_bar(stat='identity',aes(x=technique1,y=value,fill=Indic),position=position_dodge())+
   theme(axis.text.x = element_text(angle = 90, vjust = 0.5, hjust=1))
```

```
## Warning: Removed 1 rows containing missing values (`geom_bar()`).
```

<img src="/tmp/RtmpItTkgh/preview-2658e22e89d0f4.dir/Rapports_base2_files/figure-html/unnamed-chunk-15-1.png" width="672" />
### Cout d'investissement  moyen (rapporté à l'année) des U.P. en termes d'achat d'embarcation (par catégorie d'U.P.)

Diviser le coût d'achat par l'intervalle entre deux achats. L'intervalle moyen entre deux achats peut-être estimée par le double de l'ancienneté d'achat, puisque'on peut considérer que, en moyenne, une enquête se déroule à la demi-durée entre deux achats (cf. moyenne lorsqu'on tire dans une loi uniforme). 


```r
listes_site_eco.init %>% 
select(c('region_niv1','longueur','type_pirogue','annee_achat','etat_achat','prix_achat')) %>% 
  dplyr::mutate(class_long=case_when(longueur<6 ~ '<6m',longueur>=6 & longueur<12 ~'[6-12[m',longueur>=12 & longueur<20 ~'[12-20[m', longueur>=20 ~ '>=20m')) %>% 
  ggplot()+geom_boxplot(aes(x=type_pirogue,y=prix_achat))
```

<img src="/tmp/RtmpItTkgh/preview-2658e22e89d0f4.dir/Rapports_base2_files/figure-html/unnamed-chunk-16-1.png" width="672" />

```r
listes_site_eco.init %>% 
select(c('region_niv1','longueur','type_pirogue','annee_achat','etat_achat','prix_achat')) %>% 
  dplyr::mutate(class_long=case_when(longueur<6 ~ '<6m',longueur>=6 & longueur<12 ~'[6-12[m',longueur>=12 & longueur<20 ~'[12-20[m', longueur>=20 ~ '>=20m')) %>% 
  dplyr::group_by(type_pirogue,class_long) %>% 
  dplyr::summarise(prixachat.moyen=mean(prix_achat,na.rm=TRUE))->investissement.moyen
```

```
## `summarise()` has grouped output by 'type_pirogue'. You can override using the
## `.groups` argument.
```

```r
investissement.moyen %>% kbl() %>% 
  kable_styling()
```

<table class="table" style="margin-left: auto; margin-right: auto;">
 <thead>
  <tr>
   <th style="text-align:left;"> type_pirogue </th>
   <th style="text-align:left;"> class_long </th>
   <th style="text-align:right;"> prixachat.moyen </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:left;"> FIB </td>
   <td style="text-align:left;"> [12-20[m </td>
   <td style="text-align:right;"> 2783333.3 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> FIB </td>
   <td style="text-align:left;"> [6-12[m </td>
   <td style="text-align:right;"> 8000000.0 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> FIB </td>
   <td style="text-align:left;"> &lt;6m </td>
   <td style="text-align:right;"> 1500000.0 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> MEM </td>
   <td style="text-align:left;"> [12-20[m </td>
   <td style="text-align:right;"> 2122333.4 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> MEM </td>
   <td style="text-align:left;"> [6-12[m </td>
   <td style="text-align:right;"> 654741.4 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> MEM </td>
   <td style="text-align:left;"> &lt;6m </td>
   <td style="text-align:right;"> 684027.8 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> MEM </td>
   <td style="text-align:left;"> &gt;=20m </td>
   <td style="text-align:right;"> 4875000.0 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> MOA </td>
   <td style="text-align:left;"> [12-20[m </td>
   <td style="text-align:right;"> 2770000.0 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> MOA </td>
   <td style="text-align:left;"> [6-12[m </td>
   <td style="text-align:right;"> 286785.7 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> MOA </td>
   <td style="text-align:left;"> &lt;6m </td>
   <td style="text-align:right;"> 250000.0 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> MOA </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:right;"> 2500000.0 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> MON </td>
   <td style="text-align:left;"> [6-12[m </td>
   <td style="text-align:right;"> 173750.0 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> MON </td>
   <td style="text-align:left;"> &lt;6m </td>
   <td style="text-align:right;"> 0.0 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> SEM </td>
   <td style="text-align:left;"> &lt;6m </td>
   <td style="text-align:right;"> 0.0 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:left;"> [6-12[m </td>
   <td style="text-align:right;"> 350000.0 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:left;"> &lt;6m </td>
   <td style="text-align:right;"> 100000.0 </td>
  </tr>
</tbody>
</table>

```r
ggplot(investissement.moyen)+ geom_bar(stat='identity',aes(x=type_pirogue,y=prixachat.moyen,fill=class_long),position=position_dodge())
```

<img src="/tmp/RtmpItTkgh/preview-2658e22e89d0f4.dir/Rapports_base2_files/figure-html/unnamed-chunk-16-2.png" width="672" />
### Cout de maintenance moyen des U.P. en termes d'entretien de l'embarcation (par catégorie d'U.P.)

Moyenne des coûts annuels de charpentier, en compatbailisant zero lorsque pas de recours au charpentier. 



```r
listes_site_eco.init %>% 
select(c('region_niv1','type_pirogue','cout_charpentier','combien_charpentier')) %>% 
  dplyr::group_by(type_pirogue) %>% 
  dplyr::summarise(combien_charpentier.moyen=mean(combien_charpentier,na.rm=TRUE))->cout.charpent.moyen

cout.charpent.moyen %>% kbl() %>% 
  kable_styling()
```

<table class="table" style="margin-left: auto; margin-right: auto;">
 <thead>
  <tr>
   <th style="text-align:left;"> type_pirogue </th>
   <th style="text-align:right;"> combien_charpentier.moyen </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:left;"> FIB </td>
   <td style="text-align:right;"> 0.00 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> MEM </td>
   <td style="text-align:right;"> 83133.55 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> MOA </td>
   <td style="text-align:right;"> 13809.52 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> MON </td>
   <td style="text-align:right;"> 0.00 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> SEM </td>
   <td style="text-align:right;"> 0.00 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:right;"> 0.00 </td>
  </tr>
</tbody>
</table>

```r
ggplot(cout.charpent.moyen)+ geom_bar(stat='identity',aes(x=type_pirogue,y=combien_charpentier.moyen),fill='orange')+ggtitle("Cout moyen des travaux de charpente")
```

<img src="/tmp/RtmpItTkgh/preview-2658e22e89d0f4.dir/Rapports_base2_files/figure-html/unnamed-chunk-17-1.png" width="672" />

```r
listes_site_eco.init %>% 
select(c('region_niv1','type_pirogue','cout_charpentier','combien_charpentier')) %>% 
  dplyr::group_by(type_pirogue,cout_charpentier) %>% 
  dplyr::summarise(combien_travaux.char=n())->nb.trav.charpent
```

```
## `summarise()` has grouped output by 'type_pirogue'. You can override using the
## `.groups` argument.
```

```r
ggplot(nb.trav.charpent)+
  geom_bar(stat='identity',aes(x=type_pirogue,y=combien_travaux.char,fill=coalesce(cout_charpentier,'NSP')))
```

<img src="/tmp/RtmpItTkgh/preview-2658e22e89d0f4.dir/Rapports_base2_files/figure-html/unnamed-chunk-17-2.png" width="672" />

Est ce qu'il y a des diff region


```r
listes_site_eco.init %>% 
select(c('region_niv1','type_pirogue','cout_charpentier','combien_charpentier')) %>% 
  dplyr::group_by(type_pirogue,region_niv1) %>% 
  dplyr::summarise(combien_charpentier.moyen=mean(combien_charpentier,na.rm=TRUE))->cout.charpent.moyen.reg
```

```
## `summarise()` has grouped output by 'type_pirogue'. You can override using the
## `.groups` argument.
```

```r
cout.charpent.moyen.reg %>% kbl() %>% 
  kable_styling()
```

<table class="table" style="margin-left: auto; margin-right: auto;">
 <thead>
  <tr>
   <th style="text-align:left;"> type_pirogue </th>
   <th style="text-align:left;"> region_niv1 </th>
   <th style="text-align:right;"> combien_charpentier.moyen </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:left;"> FIB </td>
   <td style="text-align:left;"> Dakar </td>
   <td style="text-align:right;"> 0.00 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> FIB </td>
   <td style="text-align:left;"> Fatick </td>
   <td style="text-align:right;"> 0.00 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> FIB </td>
   <td style="text-align:left;"> Saint Louis </td>
   <td style="text-align:right;"> 0.00 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> MEM </td>
   <td style="text-align:left;"> Dakar </td>
   <td style="text-align:right;"> 51442.62 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> MEM </td>
   <td style="text-align:left;"> Fatick </td>
   <td style="text-align:right;"> 28072.29 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> MEM </td>
   <td style="text-align:left;"> Kaolack </td>
   <td style="text-align:right;"> 0.00 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> MEM </td>
   <td style="text-align:left;"> Louga </td>
   <td style="text-align:right;"> 175000.00 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> MEM </td>
   <td style="text-align:left;"> Saint Louis </td>
   <td style="text-align:right;"> 240000.00 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> MEM </td>
   <td style="text-align:left;"> Thies </td>
   <td style="text-align:right;"> 86257.14 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> MEM </td>
   <td style="text-align:left;"> Ziguinchor </td>
   <td style="text-align:right;"> 128982.14 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> MOA </td>
   <td style="text-align:left;"> Fatick </td>
   <td style="text-align:right;"> 0.00 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> MOA </td>
   <td style="text-align:left;"> Kaolack </td>
   <td style="text-align:right;"> 0.00 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> MOA </td>
   <td style="text-align:left;"> Ziguinchor </td>
   <td style="text-align:right;"> 26363.64 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> MON </td>
   <td style="text-align:left;"> Kaolack </td>
   <td style="text-align:right;"> 0.00 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> SEM </td>
   <td style="text-align:left;"> Dakar </td>
   <td style="text-align:right;"> 0.00 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:left;"> Fatick </td>
   <td style="text-align:right;"> 0.00 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:left;"> Ziguinchor </td>
   <td style="text-align:right;"> 0.00 </td>
  </tr>
</tbody>
</table>

```r
ggplot(cout.charpent.moyen.reg)+ geom_bar(stat='identity',aes(x=type_pirogue,fill=region_niv1,y=combien_charpentier.moyen),position=position_dodge())
```

<img src="/tmp/RtmpItTkgh/preview-2658e22e89d0f4.dir/Rapports_base2_files/figure-html/unnamed-chunk-18-1.png" width="672" />


### Cout moyen consentis en achat/renouvellement des materiels de sécurité, (par catégorie d'U.P.)


Sommer les couts consentis pour l'achat/renouvellement des différents matériels de sécurité lors de la dernière année.




```r
listes_site_eco.init %>%
select(c('region_niv1','longueur','remplace_gilet','cout_gilet','remplace_gps','cout_gps','remplace_feux','cout_feux','remplace_moteur2','cout_moteur2','remplace_compas','cout_compas','remplace_phramacie','cout_phramacie')) %>% 
  dplyr::filter(remplace_gilet=='Oui') %>% 
  dplyr::mutate(class_long=case_when(longueur<6 ~ '<6m',longueur>=6 & longueur<12 ~'[6-12[m',longueur>=12 & longueur<20 ~'[12-20[m', longueur>=20 ~ '>=20m')) %>% 
  dplyr::group_by(region_niv1,class_long) %>% 
  dplyr::summarise(moy.cout.gilet=mean(cout_gilet,na.rm=TRUE))->couts.gilets
```

```
## `summarise()` has grouped output by 'region_niv1'. You can override using the
## `.groups` argument.
```

```r
couts.gilets %>% kbl() %>%
  kable_styling()
```

<table class="table" style="margin-left: auto; margin-right: auto;">
 <thead>
  <tr>
   <th style="text-align:left;"> region_niv1 </th>
   <th style="text-align:left;"> class_long </th>
   <th style="text-align:right;"> moy.cout.gilet </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:left;"> Dakar </td>
   <td style="text-align:left;"> [12-20[m </td>
   <td style="text-align:right;"> 50000.000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Dakar </td>
   <td style="text-align:left;"> [6-12[m </td>
   <td style="text-align:right;"> 24366.667 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Dakar </td>
   <td style="text-align:left;"> &lt;6m </td>
   <td style="text-align:right;"> 5500.000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Dakar </td>
   <td style="text-align:left;"> &gt;=20m </td>
   <td style="text-align:right;"> 31000.000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Fatick </td>
   <td style="text-align:left;"> [12-20[m </td>
   <td style="text-align:right;"> 28333.333 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Fatick </td>
   <td style="text-align:left;"> [6-12[m </td>
   <td style="text-align:right;"> 17111.111 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Fatick </td>
   <td style="text-align:left;"> &lt;6m </td>
   <td style="text-align:right;"> 12000.000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Fatick </td>
   <td style="text-align:left;"> &gt;=20m </td>
   <td style="text-align:right;"> 100000.000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Kaolack </td>
   <td style="text-align:left;"> [12-20[m </td>
   <td style="text-align:right;"> 0.000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Kaolack </td>
   <td style="text-align:left;"> [6-12[m </td>
   <td style="text-align:right;"> 9285.714 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Louga </td>
   <td style="text-align:left;"> [12-20[m </td>
   <td style="text-align:right;"> 56000.000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Louga </td>
   <td style="text-align:left;"> &lt;6m </td>
   <td style="text-align:right;"> 60000.000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Saint Louis </td>
   <td style="text-align:left;"> [12-20[m </td>
   <td style="text-align:right;"> 77625.000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Saint Louis </td>
   <td style="text-align:left;"> [6-12[m </td>
   <td style="text-align:right;"> 27016.667 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Saint Louis </td>
   <td style="text-align:left;"> &lt;6m </td>
   <td style="text-align:right;"> 129000.000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Saint Louis </td>
   <td style="text-align:left;"> &gt;=20m </td>
   <td style="text-align:right;"> 187500.000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Thies </td>
   <td style="text-align:left;"> [12-20[m </td>
   <td style="text-align:right;"> 46200.100 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Thies </td>
   <td style="text-align:left;"> [6-12[m </td>
   <td style="text-align:right;"> 19272.727 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Thies </td>
   <td style="text-align:left;"> &lt;6m </td>
   <td style="text-align:right;"> 22500.000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Thies </td>
   <td style="text-align:left;"> &gt;=20m </td>
   <td style="text-align:right;"> 287500.000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Ziguinchor </td>
   <td style="text-align:left;"> [12-20[m </td>
   <td style="text-align:right;"> 31400.000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Ziguinchor </td>
   <td style="text-align:left;"> [6-12[m </td>
   <td style="text-align:right;"> 26544.118 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Ziguinchor </td>
   <td style="text-align:left;"> &lt;6m </td>
   <td style="text-align:right;"> 10000.000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Ziguinchor </td>
   <td style="text-align:left;"> &gt;=20m </td>
   <td style="text-align:right;"> 155000.000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Ziguinchor </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:right;"> 20000.000 </td>
  </tr>
</tbody>
</table>

```r
ggplot(couts.gilets)+ geom_bar(stat='identity',aes(x=region_niv1,fill=class_long,y=moy.cout.gilet),position=position_dodge())
```

<img src="/tmp/RtmpItTkgh/preview-2658e22e89d0f4.dir/Rapports_base2_files/figure-html/unnamed-chunk-19-1.png" width="672" />

Même chose en rajoutant la technique principale


```r
listes_site_eco.init %>%
select(c('region_niv1','technique1','longueur','remplace_gilet','cout_gilet','remplace_gps','cout_gps','remplace_feux','cout_feux','remplace_moteur2','cout_moteur2','remplace_compas','cout_compas','remplace_phramacie','cout_phramacie')) %>% 
  dplyr::filter(remplace_gilet=='Oui') %>% 
  dplyr::mutate(class_long=case_when(longueur<6 ~ '<6m',longueur>=6 & longueur<12 ~'[6-12[m',longueur>=12 & longueur<20 ~'[12-20[m', longueur>=20 ~ '>=20m')) %>% 
  dplyr::group_by(technique1,class_long) %>% 
  dplyr::summarise(moy.cout.gilet=mean(cout_gilet,na.rm=TRUE))->couts.gilets
```

```
## `summarise()` has grouped output by 'technique1'. You can override using the
## `.groups` argument.
```

```r
couts.gilets %>% kbl() %>%
  kable_styling()
```

<table class="table" style="margin-left: auto; margin-right: auto;">
 <thead>
  <tr>
   <th style="text-align:left;"> technique1 </th>
   <th style="text-align:left;"> class_long </th>
   <th style="text-align:right;"> moy.cout.gilet </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:left;"> AU </td>
   <td style="text-align:left;"> [12-20[m </td>
   <td style="text-align:right;"> 18000.000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> AU </td>
   <td style="text-align:left;"> [6-12[m </td>
   <td style="text-align:right;"> 10000.000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> AU </td>
   <td style="text-align:left;"> &lt;6m </td>
   <td style="text-align:right;"> 0.000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> EP </td>
   <td style="text-align:left;"> [6-12[m </td>
   <td style="text-align:right;"> 8333.333 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> FMCF </td>
   <td style="text-align:left;"> [12-20[m </td>
   <td style="text-align:right;"> 26727.364 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> FMCF </td>
   <td style="text-align:left;"> [6-12[m </td>
   <td style="text-align:right;"> 19382.143 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> FMCF </td>
   <td style="text-align:left;"> &lt;6m </td>
   <td style="text-align:right;"> 85250.000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> FMCF </td>
   <td style="text-align:left;"> &gt;=20m </td>
   <td style="text-align:right;"> 62500.000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> FMCS </td>
   <td style="text-align:left;"> [12-20[m </td>
   <td style="text-align:right;"> 5000.000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> FMCS </td>
   <td style="text-align:left;"> [6-12[m </td>
   <td style="text-align:right;"> 23000.000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> FMD </td>
   <td style="text-align:left;"> [12-20[m </td>
   <td style="text-align:right;"> 49500.000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> FMD </td>
   <td style="text-align:left;"> [6-12[m </td>
   <td style="text-align:right;"> 24312.500 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> FMD </td>
   <td style="text-align:left;"> &lt;6m </td>
   <td style="text-align:right;"> 29000.000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> FMD </td>
   <td style="text-align:left;"> &gt;=20m </td>
   <td style="text-align:right;"> 75000.000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> FMD </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:right;"> 20000.000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> FME </td>
   <td style="text-align:left;"> [12-20[m </td>
   <td style="text-align:right;"> 35222.222 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> FME </td>
   <td style="text-align:left;"> [6-12[m </td>
   <td style="text-align:right;"> 27500.000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> LI </td>
   <td style="text-align:left;"> [12-20[m </td>
   <td style="text-align:right;"> 14500.000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> LI </td>
   <td style="text-align:left;"> [6-12[m </td>
   <td style="text-align:right;"> 17464.286 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> LI </td>
   <td style="text-align:left;"> &lt;6m </td>
   <td style="text-align:right;"> 5000.000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> PA </td>
   <td style="text-align:left;"> [12-20[m </td>
   <td style="text-align:right;"> 45000.000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> PA </td>
   <td style="text-align:left;"> [6-12[m </td>
   <td style="text-align:right;"> 75916.667 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> PIE </td>
   <td style="text-align:left;"> [12-20[m </td>
   <td style="text-align:right;"> 85333.333 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> PIE </td>
   <td style="text-align:left;"> [6-12[m </td>
   <td style="text-align:right;"> 21000.000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> PIE </td>
   <td style="text-align:left;"> &lt;6m </td>
   <td style="text-align:right;"> 20000.000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> SP </td>
   <td style="text-align:left;"> [12-20[m </td>
   <td style="text-align:right;"> 14000.000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> SP </td>
   <td style="text-align:left;"> [6-12[m </td>
   <td style="text-align:right;"> 11500.000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> SP </td>
   <td style="text-align:left;"> &gt;=20m </td>
   <td style="text-align:right;"> 80000.000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> ST </td>
   <td style="text-align:left;"> [12-20[m </td>
   <td style="text-align:right;"> 149500.000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> ST </td>
   <td style="text-align:left;"> &gt;=20m </td>
   <td style="text-align:right;"> 220333.333 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:left;"> [12-20[m </td>
   <td style="text-align:right;"> 12500.000 </td>
  </tr>
</tbody>
</table>

```r
ggplot(couts.gilets)+ geom_bar(stat='identity',aes(x=technique1,fill=class_long,y=moy.cout.gilet),position=position_dodge())
```

<img src="/tmp/RtmpItTkgh/preview-2658e22e89d0f4.dir/Rapports_base2_files/figure-html/unnamed-chunk-20-1.png" width="672" />

```r
listes_site_eco.init %>%
select(c('region_niv1','technique1','longueur','remplace_gilet','cout_gilet','remplace_gps','cout_gps','remplace_feux','cout_feux','remplace_moteur2','cout_moteur2','remplace_compas','cout_compas','remplace_phramacie','cout_phramacie')) %>% 
  dplyr::filter(remplace_gps=='Oui') %>% 
  dplyr::mutate(class_long=case_when(longueur<6 ~ '<6m',longueur>=6 & longueur<12 ~'[6-12[m',longueur>=12 & longueur<20 ~'[12-20[m', longueur>=20 ~ '>=20m')) %>% 
  dplyr::group_by(technique1,class_long) %>% 
  dplyr::summarise(moy.cout.gps=mean(cout_gps,na.rm=TRUE))->couts.gps
```

```
## `summarise()` has grouped output by 'technique1'. You can override using the
## `.groups` argument.
```

```r
couts.gps %>% kbl() %>%
  kable_styling()
```

<table class="table" style="margin-left: auto; margin-right: auto;">
 <thead>
  <tr>
   <th style="text-align:left;"> technique1 </th>
   <th style="text-align:left;"> class_long </th>
   <th style="text-align:right;"> moy.cout.gps </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:left;"> AU </td>
   <td style="text-align:left;"> [6-12[m </td>
   <td style="text-align:right;"> 112500.00 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> AU </td>
   <td style="text-align:left;"> &lt;6m </td>
   <td style="text-align:right;"> 125000.00 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> FMCF </td>
   <td style="text-align:left;"> [12-20[m </td>
   <td style="text-align:right;"> 90625.00 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> FMCF </td>
   <td style="text-align:left;"> [6-12[m </td>
   <td style="text-align:right;"> 117894.74 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> FMCF </td>
   <td style="text-align:left;"> &lt;6m </td>
   <td style="text-align:right;"> 127500.00 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> FMCF </td>
   <td style="text-align:left;"> &gt;=20m </td>
   <td style="text-align:right;"> 90000.00 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> FMCS </td>
   <td style="text-align:left;"> [12-20[m </td>
   <td style="text-align:right;"> 70000.00 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> FMD </td>
   <td style="text-align:left;"> [12-20[m </td>
   <td style="text-align:right;"> 92454.55 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> FMD </td>
   <td style="text-align:left;"> [6-12[m </td>
   <td style="text-align:right;"> 136000.00 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> FMD </td>
   <td style="text-align:left;"> &lt;6m </td>
   <td style="text-align:right;"> 140000.00 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> FME </td>
   <td style="text-align:left;"> [12-20[m </td>
   <td style="text-align:right;"> 105000.00 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> FME </td>
   <td style="text-align:left;"> [6-12[m </td>
   <td style="text-align:right;"> 112500.00 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> FS </td>
   <td style="text-align:left;"> &gt;=20m </td>
   <td style="text-align:right;"> 125000.00 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> LI </td>
   <td style="text-align:left;"> [12-20[m </td>
   <td style="text-align:right;"> 165000.00 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> LI </td>
   <td style="text-align:left;"> [6-12[m </td>
   <td style="text-align:right;"> 107727.27 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> LI </td>
   <td style="text-align:left;"> &lt;6m </td>
   <td style="text-align:right;"> 150000.00 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> PA </td>
   <td style="text-align:left;"> [12-20[m </td>
   <td style="text-align:right;"> 130000.00 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> PA </td>
   <td style="text-align:left;"> [6-12[m </td>
   <td style="text-align:right;"> 106000.00 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> PIE </td>
   <td style="text-align:left;"> [12-20[m </td>
   <td style="text-align:right;"> 133333.33 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> PIE </td>
   <td style="text-align:left;"> [6-12[m </td>
   <td style="text-align:right;"> 126666.67 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> PIE </td>
   <td style="text-align:left;"> &lt;6m </td>
   <td style="text-align:right;"> 110000.00 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> SP </td>
   <td style="text-align:left;"> &gt;=20m </td>
   <td style="text-align:right;"> 0.00 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> ST </td>
   <td style="text-align:left;"> [12-20[m </td>
   <td style="text-align:right;"> 167000.00 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> ST </td>
   <td style="text-align:left;"> &gt;=20m </td>
   <td style="text-align:right;"> 119444.44 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:left;"> [12-20[m </td>
   <td style="text-align:right;"> 150000.00 </td>
  </tr>
</tbody>
</table>

```r
ggplot(couts.gps)+ geom_bar(stat='identity',aes(x=technique1,fill=class_long,y=moy.cout.gps),position=position_dodge())
```

<img src="/tmp/RtmpItTkgh/preview-2658e22e89d0f4.dir/Rapports_base2_files/figure-html/unnamed-chunk-21-1.png" width="672" />

Pour les moteurs de secours


```r
listes_site_eco.init %>%
select(c('region_niv1','technique1','longueur','remplace_gilet','cout_gilet','remplace_gps','cout_gps','remplace_feux','cout_feux','remplace_moteur2','cout_moteur2','remplace_compas','cout_compas','remplace_phramacie','cout_phramacie')) %>% 
  dplyr::filter(remplace_moteur2=='Oui') %>% 
  dplyr::mutate(class_long=case_when(longueur<6 ~ '<6m',longueur>=6 & longueur<12 ~'[6-12[m',longueur>=12 & longueur<20 ~'[12-20[m', longueur>=20 ~ '>=20m')) %>% 
  dplyr::group_by(technique1,class_long) %>% 
  dplyr::summarise(moy.cout_moteur2=mean(cout_moteur2,na.rm=TRUE))->cout.moteur2
```

```
## `summarise()` has grouped output by 'technique1'. You can override using the
## `.groups` argument.
```

```r
cout.moteur2 %>% kbl() %>%
  kable_styling()
```

<table class="table" style="margin-left: auto; margin-right: auto;">
 <thead>
  <tr>
   <th style="text-align:left;"> technique1 </th>
   <th style="text-align:left;"> class_long </th>
   <th style="text-align:right;"> moy.cout_moteur2 </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:left;"> FMCF </td>
   <td style="text-align:left;"> [12-20[m </td>
   <td style="text-align:right;"> 1900000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> FMCF </td>
   <td style="text-align:left;"> [6-12[m </td>
   <td style="text-align:right;"> 500000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> FMCF </td>
   <td style="text-align:left;"> &lt;6m </td>
   <td style="text-align:right;"> 440000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> FMCS </td>
   <td style="text-align:left;"> [6-12[m </td>
   <td style="text-align:right;"> 750000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> FMD </td>
   <td style="text-align:left;"> [12-20[m </td>
   <td style="text-align:right;"> 1484286 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> FMD </td>
   <td style="text-align:left;"> [6-12[m </td>
   <td style="text-align:right;"> 1450000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> FMD </td>
   <td style="text-align:left;"> &gt;=20m </td>
   <td style="text-align:right;"> 200000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> FME </td>
   <td style="text-align:left;"> [12-20[m </td>
   <td style="text-align:right;"> 600000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> LI </td>
   <td style="text-align:left;"> [12-20[m </td>
   <td style="text-align:right;"> 1283333 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> LI </td>
   <td style="text-align:left;"> [6-12[m </td>
   <td style="text-align:right;"> 1050000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> PA </td>
   <td style="text-align:left;"> [12-20[m </td>
   <td style="text-align:right;"> 2150000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> PA </td>
   <td style="text-align:left;"> [6-12[m </td>
   <td style="text-align:right;"> 600000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> PIE </td>
   <td style="text-align:left;"> [12-20[m </td>
   <td style="text-align:right;"> 1550000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> PIE </td>
   <td style="text-align:left;"> [6-12[m </td>
   <td style="text-align:right;"> 140000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> PIE </td>
   <td style="text-align:left;"> &lt;6m </td>
   <td style="text-align:right;"> 0 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> ST </td>
   <td style="text-align:left;"> [12-20[m </td>
   <td style="text-align:right;"> 2135000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> ST </td>
   <td style="text-align:left;"> &gt;=20m </td>
   <td style="text-align:right;"> 3037273 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:left;"> [12-20[m </td>
   <td style="text-align:right;"> 2500000 </td>
  </tr>
</tbody>
</table>

```r
ggplot(cout.moteur2)+ geom_bar(stat='identity',aes(x=technique1,fill=class_long,y=moy.cout_moteur2),position=position_dodge())
```

<img src="/tmp/RtmpItTkgh/preview-2658e22e89d0f4.dir/Rapports_base2_files/figure-html/unnamed-chunk-22-1.png" width="672" />

Pour les feux 


```r
listes_site_eco.init %>%
select(c('region_niv1','technique1','longueur','remplace_gilet','cout_gilet','remplace_gps','cout_gps','remplace_feux','cout_feux','remplace_moteur2','cout_moteur2','remplace_compas','cout_compas','remplace_phramacie','cout_phramacie')) %>% 
  dplyr::filter(remplace_feux=='Oui') %>% 
  dplyr::mutate(class_long=case_when(longueur<6 ~ '<6m',longueur>=6 & longueur<12 ~'[6-12[m',longueur>=12 & longueur<20 ~'[12-20[m', longueur>=20 ~ '>=20m')) %>% 
  dplyr::group_by(technique1,class_long) %>% 
  dplyr::summarise(moy.cout_feux=mean(cout_feux,na.rm=TRUE))->cout.feux
```

```
## `summarise()` has grouped output by 'technique1'. You can override using the
## `.groups` argument.
```

```r
cout.feux %>% kbl() %>%
  kable_styling()
```

<table class="table" style="margin-left: auto; margin-right: auto;">
 <thead>
  <tr>
   <th style="text-align:left;"> technique1 </th>
   <th style="text-align:left;"> class_long </th>
   <th style="text-align:right;"> moy.cout_feux </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:left;"> AU </td>
   <td style="text-align:left;"> [12-20[m </td>
   <td style="text-align:right;"> 0.000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> AU </td>
   <td style="text-align:left;"> [6-12[m </td>
   <td style="text-align:right;"> 3000.000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> FMCF </td>
   <td style="text-align:left;"> [12-20[m </td>
   <td style="text-align:right;"> 8511.111 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> FMCF </td>
   <td style="text-align:left;"> [6-12[m </td>
   <td style="text-align:right;"> 5225.000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> FMCF </td>
   <td style="text-align:left;"> &lt;6m </td>
   <td style="text-align:right;"> 7666.667 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> FMCF </td>
   <td style="text-align:left;"> &gt;=20m </td>
   <td style="text-align:right;"> 40500.000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> FMCS </td>
   <td style="text-align:left;"> [12-20[m </td>
   <td style="text-align:right;"> 5000.000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> FMCS </td>
   <td style="text-align:left;"> [6-12[m </td>
   <td style="text-align:right;"> 25333.333 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> FMD </td>
   <td style="text-align:left;"> [12-20[m </td>
   <td style="text-align:right;"> 9964.286 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> FMD </td>
   <td style="text-align:left;"> [6-12[m </td>
   <td style="text-align:right;"> 11735.294 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> FMD </td>
   <td style="text-align:left;"> &lt;6m </td>
   <td style="text-align:right;"> 30000.000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> FMD </td>
   <td style="text-align:left;"> &gt;=20m </td>
   <td style="text-align:right;"> 10000.000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> FME </td>
   <td style="text-align:left;"> [12-20[m </td>
   <td style="text-align:right;"> 13846.154 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> FME </td>
   <td style="text-align:left;"> [6-12[m </td>
   <td style="text-align:right;"> 2300.000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> FME </td>
   <td style="text-align:left;"> &gt;=20m </td>
   <td style="text-align:right;"> 90000.000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> FS </td>
   <td style="text-align:left;"> &gt;=20m </td>
   <td style="text-align:right;"> 15.000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> LI </td>
   <td style="text-align:left;"> [12-20[m </td>
   <td style="text-align:right;"> 4333.333 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> LI </td>
   <td style="text-align:left;"> [6-12[m </td>
   <td style="text-align:right;"> 4678.571 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> PA </td>
   <td style="text-align:left;"> [12-20[m </td>
   <td style="text-align:right;"> 26833.333 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> PA </td>
   <td style="text-align:left;"> [6-12[m </td>
   <td style="text-align:right;"> 28583.333 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> PIE </td>
   <td style="text-align:left;"> [12-20[m </td>
   <td style="text-align:right;"> 6000.000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> PIE </td>
   <td style="text-align:left;"> [6-12[m </td>
   <td style="text-align:right;"> 6300.000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> PIE </td>
   <td style="text-align:left;"> &lt;6m </td>
   <td style="text-align:right;"> 3000.000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> SP </td>
   <td style="text-align:left;"> [12-20[m </td>
   <td style="text-align:right;"> 3125.000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> SP </td>
   <td style="text-align:left;"> [6-12[m </td>
   <td style="text-align:right;"> 5000.000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> ST </td>
   <td style="text-align:left;"> [12-20[m </td>
   <td style="text-align:right;"> 14000.000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> ST </td>
   <td style="text-align:left;"> &gt;=20m </td>
   <td style="text-align:right;"> 5500.000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:left;"> [12-20[m </td>
   <td style="text-align:right;"> 40000.000 </td>
  </tr>
</tbody>
</table>

```r
ggplot(cout.feux)+ geom_bar(stat='identity',aes(x=technique1,fill=class_long,y=moy.cout_feux),position=position_dodge())
```

<img src="/tmp/RtmpItTkgh/preview-2658e22e89d0f4.dir/Rapports_base2_files/figure-html/unnamed-chunk-23-1.png" width="672" />

En faisant la somme de tous les éléments de securité 


```r
listes_site_eco.init %>%
select(c('region_niv1','technique1','longueur','remplace_gilet','cout_gilet','remplace_gps','cout_gps','remplace_feux','cout_feux','remplace_moteur2','cout_moteur2','remplace_compas','cout_compas','cout_moteur2','remplace_phramacie','cout_phramacie')) %>% 
  dplyr::mutate(class_long=case_when(longueur<6 ~ '<6m',longueur>=6 & longueur<12 ~'[6-12[m',longueur>=12 & longueur<20 ~'[12-20[m', longueur>=20 ~ '>=20m')) %>% 
  dplyr::group_by(technique1,class_long) %>% 
  dplyr::summarise(moy.cout_total=mean(cout_gps+cout_feux+cout_gilet+cout_moteur2+cout_compas+cout_phramacie,na.rm=TRUE))->cout.total
```

```
## `summarise()` has grouped output by 'technique1'. You can override using the
## `.groups` argument.
```

```r
cout.total %>% kbl() %>%
  kable_styling()
```

<table class="table" style="margin-left: auto; margin-right: auto;">
 <thead>
  <tr>
   <th style="text-align:left;"> technique1 </th>
   <th style="text-align:left;"> class_long </th>
   <th style="text-align:right;"> moy.cout_total </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:left;"> AU </td>
   <td style="text-align:left;"> [12-20[m </td>
   <td style="text-align:right;"> 43000.000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> AU </td>
   <td style="text-align:left;"> [6-12[m </td>
   <td style="text-align:right;"> 19888.889 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> AU </td>
   <td style="text-align:left;"> &lt;6m </td>
   <td style="text-align:right;"> 62500.000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> EP </td>
   <td style="text-align:left;"> [6-12[m </td>
   <td style="text-align:right;"> 3571.429 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> EP </td>
   <td style="text-align:left;"> &lt;6m </td>
   <td style="text-align:right;"> 0.000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> FMCF </td>
   <td style="text-align:left;"> [12-20[m </td>
   <td style="text-align:right;"> 523507.769 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> FMCF </td>
   <td style="text-align:left;"> [6-12[m </td>
   <td style="text-align:right;"> 119453.846 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> FMCF </td>
   <td style="text-align:left;"> &lt;6m </td>
   <td style="text-align:right;"> 184944.444 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> FMCF </td>
   <td style="text-align:left;"> &gt;=20m </td>
   <td style="text-align:right;"> 136000.000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> FMCS </td>
   <td style="text-align:left;"> [12-20[m </td>
   <td style="text-align:right;"> 80000.000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> FMCS </td>
   <td style="text-align:left;"> [6-12[m </td>
   <td style="text-align:right;"> 318333.333 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> FMD </td>
   <td style="text-align:left;"> [12-20[m </td>
   <td style="text-align:right;"> 367934.783 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> FMD </td>
   <td style="text-align:left;"> [6-12[m </td>
   <td style="text-align:right;"> 66681.818 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> FMD </td>
   <td style="text-align:left;"> &lt;6m </td>
   <td style="text-align:right;"> 91000.000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> FMD </td>
   <td style="text-align:left;"> &gt;=20m </td>
   <td style="text-align:right;"> 330000.000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> FMD </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:right;"> 20000.000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> FMDS </td>
   <td style="text-align:left;"> [6-12[m </td>
   <td style="text-align:right;"> 0.000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> FME </td>
   <td style="text-align:left;"> [12-20[m </td>
   <td style="text-align:right;"> 91352.941 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> FME </td>
   <td style="text-align:left;"> [6-12[m </td>
   <td style="text-align:right;"> 36500.000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> FME </td>
   <td style="text-align:left;"> &lt;6m </td>
   <td style="text-align:right;"> 0.000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> FME </td>
   <td style="text-align:left;"> &gt;=20m </td>
   <td style="text-align:right;"> 90000.000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> FS </td>
   <td style="text-align:left;"> &gt;=20m </td>
   <td style="text-align:right;"> 135015.000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> LI </td>
   <td style="text-align:left;"> [12-20[m </td>
   <td style="text-align:right;"> 667600.000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> LI </td>
   <td style="text-align:left;"> [6-12[m </td>
   <td style="text-align:right;"> 133968.750 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> LI </td>
   <td style="text-align:left;"> &lt;6m </td>
   <td style="text-align:right;"> 23888.889 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> PA </td>
   <td style="text-align:left;"> [12-20[m </td>
   <td style="text-align:right;"> 1699333.333 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> PA </td>
   <td style="text-align:left;"> [6-12[m </td>
   <td style="text-align:right;"> 135615.385 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> PA </td>
   <td style="text-align:left;"> &lt;6m </td>
   <td style="text-align:right;"> 0.000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> PIE </td>
   <td style="text-align:left;"> [12-20[m </td>
   <td style="text-align:right;"> 742666.667 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> PIE </td>
   <td style="text-align:left;"> [6-12[m </td>
   <td style="text-align:right;"> 72944.444 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> PIE </td>
   <td style="text-align:left;"> &lt;6m </td>
   <td style="text-align:right;"> 151000.000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> SP </td>
   <td style="text-align:left;"> [12-20[m </td>
   <td style="text-align:right;"> 7958.333 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> SP </td>
   <td style="text-align:left;"> [6-12[m </td>
   <td style="text-align:right;"> 10083.333 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> SP </td>
   <td style="text-align:left;"> &gt;=20m </td>
   <td style="text-align:right;"> 80000.000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> ST </td>
   <td style="text-align:left;"> [12-20[m </td>
   <td style="text-align:right;"> 788800.000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> ST </td>
   <td style="text-align:left;"> &gt;=20m </td>
   <td style="text-align:right;"> 2664428.571 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:left;"> [12-20[m </td>
   <td style="text-align:right;"> 2702500.000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:left;"> &lt;6m </td>
   <td style="text-align:right;"> 0.000 </td>
  </tr>
</tbody>
</table>

```r
ggplot(cout.total)+ geom_bar(stat='identity',aes(x=technique1,fill=class_long,y=moy.cout_total),position=position_dodge())
```

<img src="/tmp/RtmpItTkgh/preview-2658e22e89d0f4.dir/Rapports_base2_files/figure-html/unnamed-chunk-24-1.png" width="672" />



### Cout moyens de la migration de base, par type d'U.P.	

Ne faire ce calcul que pour les pirogues effectuant des migrations (changement de base) au cours de l'année. Sommer les couts consentis pour l'hebergement/cuisine et pour le transport vers les lieux de migration. Si zero, comptabiliser zero.


```r
listes_site_eco.init %>%
select(c('region_niv1','technique1','migration','cout_migration','cout_mig_carburant','cout_mig_heberge')) %>% 
filter(migration=='Oui') %>% 
  dplyr::group_by(technique1) %>% 
  dplyr::summarise(moy.cout_migration.carb=mean(cout_mig_carburant,na.rm=TRUE),moy.cout_migration.heberge=mean(cout_mig_heberge,na.rm=TRUE),nb.reponse=n())->cout.migration

cout.migration %>% kbl() %>%
  kable_styling()
```

<table class="table" style="margin-left: auto; margin-right: auto;">
 <thead>
  <tr>
   <th style="text-align:left;"> technique1 </th>
   <th style="text-align:right;"> moy.cout_migration.carb </th>
   <th style="text-align:right;"> moy.cout_migration.heberge </th>
   <th style="text-align:right;"> nb.reponse </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:left;"> AU </td>
   <td style="text-align:right;"> 15000.00 </td>
   <td style="text-align:right;"> 1666.667 </td>
   <td style="text-align:right;"> 12 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> FMCF </td>
   <td style="text-align:right;"> 130120.00 </td>
   <td style="text-align:right;"> 52884.615 </td>
   <td style="text-align:right;"> 26 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> FMCS </td>
   <td style="text-align:right;"> 26666.67 </td>
   <td style="text-align:right;"> 23333.333 </td>
   <td style="text-align:right;"> 3 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> FMD </td>
   <td style="text-align:right;"> 223565.22 </td>
   <td style="text-align:right;"> 52673.913 </td>
   <td style="text-align:right;"> 23 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> FMDS </td>
   <td style="text-align:right;"> 0.00 </td>
   <td style="text-align:right;"> 0.000 </td>
   <td style="text-align:right;"> 1 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> FME </td>
   <td style="text-align:right;"> 41111.11 </td>
   <td style="text-align:right;"> 25555.556 </td>
   <td style="text-align:right;"> 9 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> LI </td>
   <td style="text-align:right;"> 171315.79 </td>
   <td style="text-align:right;"> 56052.632 </td>
   <td style="text-align:right;"> 19 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> PA </td>
   <td style="text-align:right;"> 75000.00 </td>
   <td style="text-align:right;"> 32285.714 </td>
   <td style="text-align:right;"> 7 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> PIE </td>
   <td style="text-align:right;"> 72750.00 </td>
   <td style="text-align:right;"> 35625.000 </td>
   <td style="text-align:right;"> 8 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> SP </td>
   <td style="text-align:right;"> 30833.33 </td>
   <td style="text-align:right;"> 15833.333 </td>
   <td style="text-align:right;"> 6 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> ST </td>
   <td style="text-align:right;"> 342000.00 </td>
   <td style="text-align:right;"> 27500.000 </td>
   <td style="text-align:right;"> 10 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:right;"> 0.00 </td>
   <td style="text-align:right;"> 0.000 </td>
   <td style="text-align:right;"> 1 </td>
  </tr>
</tbody>
</table>

```r
cout.migration %>% pivot_longer(!technique1,names_to='Indic') %>% 
ggplot()+ geom_bar(stat='identity',aes(x=technique1,y=value),fill='lightgreen',position=position_dodge())+
   theme(axis.text.x = element_text(angle = 90, vjust = 0.5, hjust=1))+
  facet_wrap(~Indic,scales = 'free_y')
```

<img src="/tmp/RtmpItTkgh/preview-2658e22e89d0f4.dir/Rapports_base2_files/figure-html/unnamed-chunk-25-1.png" width="672" />

### Effort de peche par techniques 

C'est un nombre de sortie annuel par technique en multupliant le nombre de sortie par mois et le nombre de mois d'utilisation de la technique 1


```r
donnees$eco_mois_tech1 %>% 
  group_by(numero_fiche,id_unite,nom_site,date_enquete) %>% 
  summarise(nb.mois.activ.tech1=n())->activite.tech1
```

```
## `summarise()` has grouped output by 'numero_fiche', 'id_unite', 'nom_site'. You
## can override using the `.groups` argument.
```

```r
listes_site_eco.init %>% 
  inner_join(activite.tech1,by=c('numero_fiche','id_unite','nom_site','date_enquete'))%>%
select(c('region_niv1','technique1','nb.mois.activ.tech1','nb_sortie_mois_tech1')) %>% 
  dplyr::group_by(technique1) %>% 
  dplyr::summarise(nb.sortie.annuel=mean(nb.mois.activ.tech1*nb_sortie_mois_tech1,na.rm=TRUE),nb.reponse=n())->effort.annuel

effort.annuel %>% kbl() %>%
  kable_styling()
```

<table class="table" style="margin-left: auto; margin-right: auto;">
 <thead>
  <tr>
   <th style="text-align:left;"> technique1 </th>
   <th style="text-align:right;"> nb.sortie.annuel </th>
   <th style="text-align:right;"> nb.reponse </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:left;"> AU </td>
   <td style="text-align:right;"> 277.7619 </td>
   <td style="text-align:right;"> 21 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> EP </td>
   <td style="text-align:right;"> 303.7500 </td>
   <td style="text-align:right;"> 8 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> FMCF </td>
   <td style="text-align:right;"> 100.6984 </td>
   <td style="text-align:right;"> 63 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> FMCS </td>
   <td style="text-align:right;"> 123.0000 </td>
   <td style="text-align:right;"> 4 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> FMD </td>
   <td style="text-align:right;"> 108.9506 </td>
   <td style="text-align:right;"> 81 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> FMDS </td>
   <td style="text-align:right;"> 52.0000 </td>
   <td style="text-align:right;"> 1 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> FME </td>
   <td style="text-align:right;"> 126.8750 </td>
   <td style="text-align:right;"> 32 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> FS </td>
   <td style="text-align:right;"> 18.0000 </td>
   <td style="text-align:right;"> 1 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> LI </td>
   <td style="text-align:right;"> 106.6522 </td>
   <td style="text-align:right;"> 46 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> PA </td>
   <td style="text-align:right;"> 101.0556 </td>
   <td style="text-align:right;"> 18 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> PIE </td>
   <td style="text-align:right;"> 135.6923 </td>
   <td style="text-align:right;"> 13 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> SP </td>
   <td style="text-align:right;"> 121.5652 </td>
   <td style="text-align:right;"> 24 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> ST </td>
   <td style="text-align:right;"> 116.4444 </td>
   <td style="text-align:right;"> 18 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:right;"> 48.0000 </td>
   <td style="text-align:right;"> 1 </td>
  </tr>
</tbody>
</table>

```r
ggplot(effort.annuel)+ geom_bar(stat='identity',aes(x=technique1,y=nb.sortie.annuel),fill='lightgreen',position=position_dodge())+
   theme(axis.text.x = element_text(angle = 90, vjust = 0.5, hjust=1))
```

<img src="/tmp/RtmpItTkgh/preview-2658e22e89d0f4.dir/Rapports_base2_files/figure-html/unnamed-chunk-26-1.png" width="672" />
C'est un nombre de sortie annuel par technique en multupliant le nombre de sortie par mois et le nombre de mois d'utilisation de la technique 2


```r
donnees$eco_mois_tech2 %>% 
  group_by(numero_fiche,id_unite,nom_site,date_enquete) %>% 
  summarise(nb.mois.activ.tech2=n())->activite.tech2
```

```
## `summarise()` has grouped output by 'numero_fiche', 'id_unite', 'nom_site'. You
## can override using the `.groups` argument.
```

```r
listes_site_eco.init %>% 
  inner_join(activite.tech2,by=c('numero_fiche','id_unite','nom_site','date_enquete'))%>%
select(c('region_niv1','technique2','nb.mois.activ.tech2','nb_sortie_mois_tech2')) %>% 
  dplyr::group_by(technique2) %>% 
  dplyr::summarise(nb.sortie.annuel=mean(nb.mois.activ.tech2*nb_sortie_mois_tech2,na.rm=TRUE),nb.reponse=n())->effort.annuel.tech2

effort.annuel.tech2 %>% kbl() %>%
  kable_styling()
```

<table class="table" style="margin-left: auto; margin-right: auto;">
 <thead>
  <tr>
   <th style="text-align:left;"> technique2 </th>
   <th style="text-align:right;"> nb.sortie.annuel </th>
   <th style="text-align:right;"> nb.reponse </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:left;"> AU </td>
   <td style="text-align:right;"> 69.00000 </td>
   <td style="text-align:right;"> 4 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> FMCF </td>
   <td style="text-align:right;"> 75.69231 </td>
   <td style="text-align:right;"> 26 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> FMCS </td>
   <td style="text-align:right;"> 59.50000 </td>
   <td style="text-align:right;"> 4 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> FMD </td>
   <td style="text-align:right;"> 77.34286 </td>
   <td style="text-align:right;"> 35 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> FME </td>
   <td style="text-align:right;"> 35.00000 </td>
   <td style="text-align:right;"> 4 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> LI </td>
   <td style="text-align:right;"> 63.45161 </td>
   <td style="text-align:right;"> 31 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> PA </td>
   <td style="text-align:right;"> 77.55556 </td>
   <td style="text-align:right;"> 9 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> PIE </td>
   <td style="text-align:right;"> 85.00000 </td>
   <td style="text-align:right;"> 2 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> SP </td>
   <td style="text-align:right;"> 80.00000 </td>
   <td style="text-align:right;"> 6 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:right;"> 72.00000 </td>
   <td style="text-align:right;"> 5 </td>
  </tr>
</tbody>
</table>

```r
ggplot(effort.annuel.tech2)+ geom_bar(stat='identity',aes(x=technique2,y=nb.sortie.annuel),fill='lightblue',position=position_dodge())+
   theme(axis.text.x = element_text(angle = 90, vjust = 0.5, hjust=1))
```

<img src="/tmp/RtmpItTkgh/preview-2658e22e89d0f4.dir/Rapports_base2_files/figure-html/unnamed-chunk-27-1.png" width="672" />

## Module de transformation

### Informations générales sur les sites enquêtés


```
## Warning in mask$eval_all_mutate(quo): NAs introduits lors de la conversion
## automatique

## Warning in mask$eval_all_mutate(quo): NAs introduits lors de la conversion
## automatique
```
### Nombre de site de Transformation Débarquement par communes/Départements côtièrs

```
## `summarise()` has grouped output by 'Région'. You can override using the
## `.groups` argument.
```

<table class="table" style="margin-left: auto; margin-right: auto;">
 <thead>
  <tr>
   <th style="text-align:left;"> Région </th>
   <th style="text-align:left;"> Departement </th>
   <th style="text-align:right;"> Site_transfo </th>
   <th style="text-align:right;"> pourcentage.site_debar </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:left;"> Dakar </td>
   <td style="text-align:left;"> Dakar </td>
   <td style="text-align:right;"> 4 </td>
   <td style="text-align:right;"> 4.494382 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Dakar </td>
   <td style="text-align:left;"> Rufisque </td>
   <td style="text-align:right;"> 11 </td>
   <td style="text-align:right;"> 12.359551 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Fatick </td>
   <td style="text-align:left;"> Fatick </td>
   <td style="text-align:right;"> 6 </td>
   <td style="text-align:right;"> 6.741573 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Fatick </td>
   <td style="text-align:left;"> Foundiougne </td>
   <td style="text-align:right;"> 23 </td>
   <td style="text-align:right;"> 25.842697 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Kaolack </td>
   <td style="text-align:left;"> Kaolack </td>
   <td style="text-align:right;"> 3 </td>
   <td style="text-align:right;"> 3.370786 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Louga </td>
   <td style="text-align:left;"> Louga </td>
   <td style="text-align:right;"> 3 </td>
   <td style="text-align:right;"> 3.370786 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Saint Louis </td>
   <td style="text-align:left;"> Saint Louis </td>
   <td style="text-align:right;"> 12 </td>
   <td style="text-align:right;"> 13.483146 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Saint Louis </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 1.123595 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Thies </td>
   <td style="text-align:left;"> Mbour </td>
   <td style="text-align:right;"> 12 </td>
   <td style="text-align:right;"> 13.483146 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Thies </td>
   <td style="text-align:left;"> Thies </td>
   <td style="text-align:right;"> 4 </td>
   <td style="text-align:right;"> 4.494382 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Thies </td>
   <td style="text-align:left;"> Tivaoune </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 1.123595 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Ziguinchor </td>
   <td style="text-align:left;"> Bignona </td>
   <td style="text-align:right;"> 3 </td>
   <td style="text-align:right;"> 3.370786 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Ziguinchor </td>
   <td style="text-align:left;"> Oussouye </td>
   <td style="text-align:right;"> 3 </td>
   <td style="text-align:right;"> 3.370786 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Ziguinchor </td>
   <td style="text-align:left;"> Ziguinchor </td>
   <td style="text-align:right;"> 3 </td>
   <td style="text-align:right;"> 3.370786 </td>
  </tr>
</tbody>
</table>




```r
leaflet(liste.sites.init %>% filter(latitude!=0 ) %>% 
  dplyr::mutate(latitude=as.numeric(latitude),longitude=as.numeric(longitude))) %>% addProviderTiles("Esri.WorldImagery")  %>%
  addCircleMarkers(radius = 10,
    color = 'red',
      stroke = FALSE, fillOpacity = 0.5) %>% 
   addLabelOnlyMarkers(~longitude,~latitude, label =  ~as.character(code_site), 
                      labelOptions = labelOptions(noHide = T, direction = 'top', textOnly = T))
```

```
## Assuming "longitude" and "latitude" are longitude and latitude, respectively
```

```{=html}
<div class="leaflet html-widget html-fill-item-overflow-hidden html-fill-item" id="htmlwidget-adcd368886dd39514725" style="width:672px;height:480px;"></div>
<script type="application/json" data-for="htmlwidget-adcd368886dd39514725">{"x":{"options":{"crs":{"crsClass":"L.CRS.EPSG3857","code":null,"proj4def":null,"projectedBounds":null,"options":{}}},"calls":[{"method":"addProviderTiles","args":["Esri.WorldImagery",null,null,{"errorTileUrl":"","noWrap":false,"detectRetina":false}]},{"method":"addCircleMarkers","args":[14,-17,10,null,null,{"interactive":true,"className":"","stroke":false,"color":"red","weight":5,"opacity":0.5,"fill":true,"fillColor":"red","fillOpacity":0.5},null,null,null,null,null,{"interactive":false,"permanent":false,"direction":"auto","opacity":1,"offset":[0,0],"textsize":"10px","textOnly":false,"className":"","sticky":true},null]},{"method":"addMarkers","args":[14,-17,{"iconUrl":{"data":"data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAQAAAC1HAwCAAAAC0lEQVR4nGP6zwAAAgcBApocMXEAAAAASUVORK5CYII=","index":0},"iconWidth":1,"iconHeight":1},null,null,{"interactive":true,"draggable":false,"keyboard":true,"title":"","alt":"","zIndexOffset":0,"opacity":1,"riseOnHover":false,"riseOffset":250},null,null,null,null,"406",{"interactive":false,"permanent":true,"direction":"top","opacity":1,"offset":[0,0],"textsize":"10px","textOnly":true,"className":"","sticky":true},null]}],"limits":{"lat":[14,14],"lng":[-17,-17]}},"evals":[],"jsHooks":[]}</script>
```

### Par régions, Site de transformation liés au site de débarquement 


```r
names(donnees$transfo_sites)
```

```
##  [1] "num_fiche_enq"                    "date_enq"                        
##  [3] "enqueteur"                        "superviseur_enq"                 
##  [5] "personne_enqte_01"                "fonction_01"                     
##  [7] "personne_enqte_02"                "fonction_02"                     
##  [9] "personne_enqte_03"                "fonction_03"                     
## [11] "pays"                             "niveau_administr_01"             
## [13] "niveau_administr_02"              "localite_rattach_adm"            
## [15] "code_site"                        "lat_dd_mm_ss"                    
## [17] "long_dd_mm_ss"                    "lat_deg_deci"                    
## [19] "long_deg_deci"                    "site_transf_debarq"              
## [21] "site01_provenance_pois"           "site02_provenance_pois"          
## [23] "site03_provenance_pois"           "exist_pois_importe"              
## [25] "nbr_total_hom_transf"             "nbr_total_fem_transf"            
## [27] "nbr_hom_transformateur"           "nbr_fem_transformatrice"         
## [29] "commodites_associe"               "site_amenage_am"                 
## [31] "certificat_site"                  "robinet"                         
## [33] "latrine_douche"                   "parking"                         
## [35] "magasinage_prod_site"             "approv_combust"                  
## [37] "ramassage_traitem_dechets"        "boutiq_mat_transf"               
## [39] "electricite_public"               "couverture_telephone"            
## [41] "couverture_internet"              "nb_commercat_venu_sem"           
## [43] "qte_pois_transf_import_sem"       "qte_importe"                     
## [45] "operateur_transf"                 "personne_cherche_pois_transf"    
## [47] "combien_operateur_activite"       "taille_grp_echantillon_operateur"
## [49] "combien_person_pois_transf"       "taille_grp_echantillon_personne" 
## [51] "date_validation"                  "date_saisie"                     
## [53] "nom_agent_saisie"
```

```r
donnees$transfo_sites %>% 
  mutate(Région=niveau_administr_01) %>%
  select(Région,code_site,site_transf_debarq) %>% 
  group_by(Région,site_transf_debarq) %>% 
  summarize(nb_sites=n()) %>% 
  ggplot()+geom_bar(stat='identity',aes(x=Région,fill=site_transf_debarq,
                                        y=nb_sites),position=position_dodge())
```

```
## `summarise()` has grouped output by 'Région'. You can override using the
## `.groups` argument.
```

<img src="/tmp/RtmpItTkgh/preview-2658e22e89d0f4.dir/Rapports_base2_files/figure-html/unnamed-chunk-31-1.png" width="672" />



```r
donnees$transfo_sites[,c(12,seq(25,28))] %>% 
       pivot_longer( !niveau_administr_01, names_to = "Emploi") %>% 
  mutate(Région=niveau_administr_01) %>%
  group_by(Région,Emploi) %>% summarise(value=sum(value,na.rm=TRUE)) %>% 
  ggplot()+geom_bar(stat='identity',aes(x=Région,fill=Emploi,y=value),position=position_dodge())
```

```
## `summarise()` has grouped output by 'Région'. You can override using the
## `.groups` argument.
```

<img src="/tmp/RtmpItTkgh/preview-2658e22e89d0f4.dir/Rapports_base2_files/figure-html/unnamed-chunk-32-1.png" width="672" />

### Commodités Présent et fonctionne Pays


```r
total<-dim(liste.sites.init)[1]

liste.sites.init[,c(1,14,seq(30,41))]  %>%
  pivot_longer( cols = names(liste.sites)[c(seq(30,41))], names_to = "Commodites") %>%
  filter(value=='Présent et fonctionne') %>%
  group_by(Commodites) %>%
  summarize(Site_transf=n())%>%
 mutate(pour.Site_transf=Site_transf/total*100) -> commodites

commodites %>%
  kbl() %>% kable_styling()
```

<table class="table" style="margin-left: auto; margin-right: auto;">
 <thead>
  <tr>
   <th style="text-align:left;"> Commodites </th>
   <th style="text-align:right;"> Site_transf </th>
   <th style="text-align:right;"> pour.Site_transf </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:left;"> approv_combust </td>
   <td style="text-align:right;"> 25 </td>
   <td style="text-align:right;"> 28.089888 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> boutiq_mat_transf </td>
   <td style="text-align:right;"> 4 </td>
   <td style="text-align:right;"> 4.494382 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> certificat_site </td>
   <td style="text-align:right;"> 58 </td>
   <td style="text-align:right;"> 65.168539 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> couverture_internet </td>
   <td style="text-align:right;"> 62 </td>
   <td style="text-align:right;"> 69.662921 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> couverture_telephone </td>
   <td style="text-align:right;"> 64 </td>
   <td style="text-align:right;"> 71.910112 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> electricite_public </td>
   <td style="text-align:right;"> 35 </td>
   <td style="text-align:right;"> 39.325843 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> latrine_douche </td>
   <td style="text-align:right;"> 34 </td>
   <td style="text-align:right;"> 38.202247 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> magasinage_prod_site </td>
   <td style="text-align:right;"> 27 </td>
   <td style="text-align:right;"> 30.337079 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> parking </td>
   <td style="text-align:right;"> 10 </td>
   <td style="text-align:right;"> 11.235955 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> ramassage_traitem_dechets </td>
   <td style="text-align:right;"> 25 </td>
   <td style="text-align:right;"> 28.089888 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> robinet </td>
   <td style="text-align:right;"> 23 </td>
   <td style="text-align:right;"> 25.842697 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> site_amenage_am </td>
   <td style="text-align:right;"> 33 </td>
   <td style="text-align:right;"> 37.078652 </td>
  </tr>
</tbody>
</table>



```r
ggplot(commodites)+
  geom_bar(stat='identity',aes(x=Commodites,y=pour.Site_transf))+
   theme(axis.text.x = element_text(angle = 90, vjust = 0.5, hjust=1))
```

<img src="/tmp/RtmpItTkgh/preview-2658e22e89d0f4.dir/Rapports_base2_files/figure-html/unnamed-chunk-34-1.png" width="672" />

### Commodités Présent et fonctionne Région


```r
total.reg<- liste.sites.init %>% 
  group_by(niveau_administr_01) %>%
  dplyr::summarise(total.reg=n())


liste.sites.init[,c(1,12,14,seq(30,41))]  %>%
  pivot_longer( cols = names(liste.sites)[c(seq(30,41))], names_to = "Commodites") %>%
  filter(value=='Présent et fonctionne') %>%
  group_by(niveau_administr_01,Commodites) %>%
  summarize(nb.sites=n()) %>% inner_join(total.reg) %>%
   dplyr::mutate(pour.Site_transf=100*nb.sites/total.reg)->commodites.reg
```

```
## `summarise()` has grouped output by 'niveau_administr_01'. You can override
## using the `.groups` argument.
## Joining, by = "niveau_administr_01"
```

```r
commodites.reg %>%
  kbl() %>% kable_styling()
```

<table class="table" style="margin-left: auto; margin-right: auto;">
 <thead>
  <tr>
   <th style="text-align:left;"> niveau_administr_01 </th>
   <th style="text-align:left;"> Commodites </th>
   <th style="text-align:right;"> nb.sites </th>
   <th style="text-align:right;"> total.reg </th>
   <th style="text-align:right;"> pour.Site_transf </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:left;"> Dakar </td>
   <td style="text-align:left;"> approv_combust </td>
   <td style="text-align:right;"> 8 </td>
   <td style="text-align:right;"> 15 </td>
   <td style="text-align:right;"> 53.333333 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Dakar </td>
   <td style="text-align:left;"> boutiq_mat_transf </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 15 </td>
   <td style="text-align:right;"> 6.666667 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Dakar </td>
   <td style="text-align:left;"> certificat_site </td>
   <td style="text-align:right;"> 14 </td>
   <td style="text-align:right;"> 15 </td>
   <td style="text-align:right;"> 93.333333 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Dakar </td>
   <td style="text-align:left;"> couverture_internet </td>
   <td style="text-align:right;"> 14 </td>
   <td style="text-align:right;"> 15 </td>
   <td style="text-align:right;"> 93.333333 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Dakar </td>
   <td style="text-align:left;"> couverture_telephone </td>
   <td style="text-align:right;"> 14 </td>
   <td style="text-align:right;"> 15 </td>
   <td style="text-align:right;"> 93.333333 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Dakar </td>
   <td style="text-align:left;"> electricite_public </td>
   <td style="text-align:right;"> 15 </td>
   <td style="text-align:right;"> 15 </td>
   <td style="text-align:right;"> 100.000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Dakar </td>
   <td style="text-align:left;"> latrine_douche </td>
   <td style="text-align:right;"> 4 </td>
   <td style="text-align:right;"> 15 </td>
   <td style="text-align:right;"> 26.666667 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Dakar </td>
   <td style="text-align:left;"> magasinage_prod_site </td>
   <td style="text-align:right;"> 10 </td>
   <td style="text-align:right;"> 15 </td>
   <td style="text-align:right;"> 66.666667 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Dakar </td>
   <td style="text-align:left;"> parking </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 15 </td>
   <td style="text-align:right;"> 6.666667 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Dakar </td>
   <td style="text-align:left;"> ramassage_traitem_dechets </td>
   <td style="text-align:right;"> 13 </td>
   <td style="text-align:right;"> 15 </td>
   <td style="text-align:right;"> 86.666667 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Dakar </td>
   <td style="text-align:left;"> robinet </td>
   <td style="text-align:right;"> 4 </td>
   <td style="text-align:right;"> 15 </td>
   <td style="text-align:right;"> 26.666667 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Dakar </td>
   <td style="text-align:left;"> site_amenage_am </td>
   <td style="text-align:right;"> 9 </td>
   <td style="text-align:right;"> 15 </td>
   <td style="text-align:right;"> 60.000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Fatick </td>
   <td style="text-align:left;"> approv_combust </td>
   <td style="text-align:right;"> 5 </td>
   <td style="text-align:right;"> 29 </td>
   <td style="text-align:right;"> 17.241379 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Fatick </td>
   <td style="text-align:left;"> boutiq_mat_transf </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 29 </td>
   <td style="text-align:right;"> 3.448276 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Fatick </td>
   <td style="text-align:left;"> certificat_site </td>
   <td style="text-align:right;"> 14 </td>
   <td style="text-align:right;"> 29 </td>
   <td style="text-align:right;"> 48.275862 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Fatick </td>
   <td style="text-align:left;"> couverture_internet </td>
   <td style="text-align:right;"> 16 </td>
   <td style="text-align:right;"> 29 </td>
   <td style="text-align:right;"> 55.172414 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Fatick </td>
   <td style="text-align:left;"> couverture_telephone </td>
   <td style="text-align:right;"> 16 </td>
   <td style="text-align:right;"> 29 </td>
   <td style="text-align:right;"> 55.172414 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Fatick </td>
   <td style="text-align:left;"> electricite_public </td>
   <td style="text-align:right;"> 5 </td>
   <td style="text-align:right;"> 29 </td>
   <td style="text-align:right;"> 17.241379 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Fatick </td>
   <td style="text-align:left;"> latrine_douche </td>
   <td style="text-align:right;"> 10 </td>
   <td style="text-align:right;"> 29 </td>
   <td style="text-align:right;"> 34.482759 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Fatick </td>
   <td style="text-align:left;"> magasinage_prod_site </td>
   <td style="text-align:right;"> 6 </td>
   <td style="text-align:right;"> 29 </td>
   <td style="text-align:right;"> 20.689655 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Fatick </td>
   <td style="text-align:left;"> parking </td>
   <td style="text-align:right;"> 4 </td>
   <td style="text-align:right;"> 29 </td>
   <td style="text-align:right;"> 13.793103 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Fatick </td>
   <td style="text-align:left;"> ramassage_traitem_dechets </td>
   <td style="text-align:right;"> 4 </td>
   <td style="text-align:right;"> 29 </td>
   <td style="text-align:right;"> 13.793103 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Fatick </td>
   <td style="text-align:left;"> robinet </td>
   <td style="text-align:right;"> 7 </td>
   <td style="text-align:right;"> 29 </td>
   <td style="text-align:right;"> 24.137931 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Fatick </td>
   <td style="text-align:left;"> site_amenage_am </td>
   <td style="text-align:right;"> 8 </td>
   <td style="text-align:right;"> 29 </td>
   <td style="text-align:right;"> 27.586207 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Kaolack </td>
   <td style="text-align:left;"> approv_combust </td>
   <td style="text-align:right;"> 3 </td>
   <td style="text-align:right;"> 3 </td>
   <td style="text-align:right;"> 100.000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Kaolack </td>
   <td style="text-align:left;"> boutiq_mat_transf </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 3 </td>
   <td style="text-align:right;"> 33.333333 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Kaolack </td>
   <td style="text-align:left;"> couverture_internet </td>
   <td style="text-align:right;"> 3 </td>
   <td style="text-align:right;"> 3 </td>
   <td style="text-align:right;"> 100.000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Kaolack </td>
   <td style="text-align:left;"> couverture_telephone </td>
   <td style="text-align:right;"> 3 </td>
   <td style="text-align:right;"> 3 </td>
   <td style="text-align:right;"> 100.000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Kaolack </td>
   <td style="text-align:left;"> electricite_public </td>
   <td style="text-align:right;"> 3 </td>
   <td style="text-align:right;"> 3 </td>
   <td style="text-align:right;"> 100.000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Kaolack </td>
   <td style="text-align:left;"> latrine_douche </td>
   <td style="text-align:right;"> 3 </td>
   <td style="text-align:right;"> 3 </td>
   <td style="text-align:right;"> 100.000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Kaolack </td>
   <td style="text-align:left;"> magasinage_prod_site </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 3 </td>
   <td style="text-align:right;"> 33.333333 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Kaolack </td>
   <td style="text-align:left;"> parking </td>
   <td style="text-align:right;"> 3 </td>
   <td style="text-align:right;"> 3 </td>
   <td style="text-align:right;"> 100.000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Kaolack </td>
   <td style="text-align:left;"> site_amenage_am </td>
   <td style="text-align:right;"> 3 </td>
   <td style="text-align:right;"> 3 </td>
   <td style="text-align:right;"> 100.000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Louga </td>
   <td style="text-align:left;"> approv_combust </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 3 </td>
   <td style="text-align:right;"> 33.333333 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Louga </td>
   <td style="text-align:left;"> certificat_site </td>
   <td style="text-align:right;"> 3 </td>
   <td style="text-align:right;"> 3 </td>
   <td style="text-align:right;"> 100.000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Louga </td>
   <td style="text-align:left;"> couverture_internet </td>
   <td style="text-align:right;"> 3 </td>
   <td style="text-align:right;"> 3 </td>
   <td style="text-align:right;"> 100.000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Louga </td>
   <td style="text-align:left;"> couverture_telephone </td>
   <td style="text-align:right;"> 3 </td>
   <td style="text-align:right;"> 3 </td>
   <td style="text-align:right;"> 100.000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Louga </td>
   <td style="text-align:left;"> electricite_public </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 3 </td>
   <td style="text-align:right;"> 33.333333 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Louga </td>
   <td style="text-align:left;"> latrine_douche </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 3 </td>
   <td style="text-align:right;"> 33.333333 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Louga </td>
   <td style="text-align:left;"> magasinage_prod_site </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 3 </td>
   <td style="text-align:right;"> 33.333333 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Louga </td>
   <td style="text-align:left;"> ramassage_traitem_dechets </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 3 </td>
   <td style="text-align:right;"> 33.333333 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Louga </td>
   <td style="text-align:left;"> site_amenage_am </td>
   <td style="text-align:right;"> 2 </td>
   <td style="text-align:right;"> 3 </td>
   <td style="text-align:right;"> 66.666667 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Saint Louis </td>
   <td style="text-align:left;"> approv_combust </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 13 </td>
   <td style="text-align:right;"> 7.692308 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Saint Louis </td>
   <td style="text-align:left;"> certificat_site </td>
   <td style="text-align:right;"> 9 </td>
   <td style="text-align:right;"> 13 </td>
   <td style="text-align:right;"> 69.230769 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Saint Louis </td>
   <td style="text-align:left;"> couverture_internet </td>
   <td style="text-align:right;"> 8 </td>
   <td style="text-align:right;"> 13 </td>
   <td style="text-align:right;"> 61.538461 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Saint Louis </td>
   <td style="text-align:left;"> couverture_telephone </td>
   <td style="text-align:right;"> 8 </td>
   <td style="text-align:right;"> 13 </td>
   <td style="text-align:right;"> 61.538461 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Saint Louis </td>
   <td style="text-align:left;"> electricite_public </td>
   <td style="text-align:right;"> 5 </td>
   <td style="text-align:right;"> 13 </td>
   <td style="text-align:right;"> 38.461539 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Saint Louis </td>
   <td style="text-align:left;"> latrine_douche </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 13 </td>
   <td style="text-align:right;"> 7.692308 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Saint Louis </td>
   <td style="text-align:left;"> magasinage_prod_site </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 13 </td>
   <td style="text-align:right;"> 7.692308 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Saint Louis </td>
   <td style="text-align:left;"> parking </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 13 </td>
   <td style="text-align:right;"> 7.692308 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Saint Louis </td>
   <td style="text-align:left;"> site_amenage_am </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 13 </td>
   <td style="text-align:right;"> 7.692308 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Thies </td>
   <td style="text-align:left;"> approv_combust </td>
   <td style="text-align:right;"> 4 </td>
   <td style="text-align:right;"> 17 </td>
   <td style="text-align:right;"> 23.529412 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Thies </td>
   <td style="text-align:left;"> certificat_site </td>
   <td style="text-align:right;"> 12 </td>
   <td style="text-align:right;"> 17 </td>
   <td style="text-align:right;"> 70.588235 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Thies </td>
   <td style="text-align:left;"> couverture_internet </td>
   <td style="text-align:right;"> 15 </td>
   <td style="text-align:right;"> 17 </td>
   <td style="text-align:right;"> 88.235294 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Thies </td>
   <td style="text-align:left;"> couverture_telephone </td>
   <td style="text-align:right;"> 15 </td>
   <td style="text-align:right;"> 17 </td>
   <td style="text-align:right;"> 88.235294 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Thies </td>
   <td style="text-align:left;"> electricite_public </td>
   <td style="text-align:right;"> 5 </td>
   <td style="text-align:right;"> 17 </td>
   <td style="text-align:right;"> 29.411765 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Thies </td>
   <td style="text-align:left;"> latrine_douche </td>
   <td style="text-align:right;"> 12 </td>
   <td style="text-align:right;"> 17 </td>
   <td style="text-align:right;"> 70.588235 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Thies </td>
   <td style="text-align:left;"> magasinage_prod_site </td>
   <td style="text-align:right;"> 6 </td>
   <td style="text-align:right;"> 17 </td>
   <td style="text-align:right;"> 35.294118 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Thies </td>
   <td style="text-align:left;"> parking </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 17 </td>
   <td style="text-align:right;"> 5.882353 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Thies </td>
   <td style="text-align:left;"> ramassage_traitem_dechets </td>
   <td style="text-align:right;"> 6 </td>
   <td style="text-align:right;"> 17 </td>
   <td style="text-align:right;"> 35.294118 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Thies </td>
   <td style="text-align:left;"> robinet </td>
   <td style="text-align:right;"> 9 </td>
   <td style="text-align:right;"> 17 </td>
   <td style="text-align:right;"> 52.941176 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Thies </td>
   <td style="text-align:left;"> site_amenage_am </td>
   <td style="text-align:right;"> 7 </td>
   <td style="text-align:right;"> 17 </td>
   <td style="text-align:right;"> 41.176471 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Ziguinchor </td>
   <td style="text-align:left;"> approv_combust </td>
   <td style="text-align:right;"> 3 </td>
   <td style="text-align:right;"> 9 </td>
   <td style="text-align:right;"> 33.333333 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Ziguinchor </td>
   <td style="text-align:left;"> boutiq_mat_transf </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 9 </td>
   <td style="text-align:right;"> 11.111111 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Ziguinchor </td>
   <td style="text-align:left;"> certificat_site </td>
   <td style="text-align:right;"> 6 </td>
   <td style="text-align:right;"> 9 </td>
   <td style="text-align:right;"> 66.666667 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Ziguinchor </td>
   <td style="text-align:left;"> couverture_internet </td>
   <td style="text-align:right;"> 3 </td>
   <td style="text-align:right;"> 9 </td>
   <td style="text-align:right;"> 33.333333 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Ziguinchor </td>
   <td style="text-align:left;"> couverture_telephone </td>
   <td style="text-align:right;"> 5 </td>
   <td style="text-align:right;"> 9 </td>
   <td style="text-align:right;"> 55.555556 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Ziguinchor </td>
   <td style="text-align:left;"> electricite_public </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 9 </td>
   <td style="text-align:right;"> 11.111111 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Ziguinchor </td>
   <td style="text-align:left;"> latrine_douche </td>
   <td style="text-align:right;"> 3 </td>
   <td style="text-align:right;"> 9 </td>
   <td style="text-align:right;"> 33.333333 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Ziguinchor </td>
   <td style="text-align:left;"> magasinage_prod_site </td>
   <td style="text-align:right;"> 2 </td>
   <td style="text-align:right;"> 9 </td>
   <td style="text-align:right;"> 22.222222 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Ziguinchor </td>
   <td style="text-align:left;"> ramassage_traitem_dechets </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 9 </td>
   <td style="text-align:right;"> 11.111111 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Ziguinchor </td>
   <td style="text-align:left;"> robinet </td>
   <td style="text-align:right;"> 3 </td>
   <td style="text-align:right;"> 9 </td>
   <td style="text-align:right;"> 33.333333 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Ziguinchor </td>
   <td style="text-align:left;"> site_amenage_am </td>
   <td style="text-align:right;"> 3 </td>
   <td style="text-align:right;"> 9 </td>
   <td style="text-align:right;"> 33.333333 </td>
  </tr>
</tbody>
</table>


```r
ggplot(commodites.reg)+
  geom_bar(stat='identity',aes(x=Commodites,y=pour.Site_transf))+
  facet_wrap(~niveau_administr_01)+
   theme(axis.text.x = element_text(angle = 90, vjust = 0.5, hjust=1))
```

<img src="/tmp/RtmpItTkgh/preview-2658e22e89d0f4.dir/Rapports_base2_files/figure-html/unnamed-chunk-36-1.png" width="672" />

### Commodités Existe mais ne  fonctionne pas bien ou pas du tout Pays


```r
total<-dim(liste.sites.init)[1]

liste.sites.init[,c(1,14,seq(30,41))]  %>%
  pivot_longer( cols = names(liste.sites)[c(seq(30,41))], names_to = "Commodites") %>%
  filter(value=='Existe mais ne  fonctionne pas bien ou pas du tout') %>%
  group_by(Commodites) %>%
  summarize(Site_transf=n())%>%
 mutate(pour.Site_transf=Site_transf/total*100) -> commodites

commodites %>%
  kbl() %>% kable_styling()
```

<table class="table" style="margin-left: auto; margin-right: auto;">
 <thead>
  <tr>
   <th style="text-align:left;"> Commodites </th>
   <th style="text-align:right;"> Site_transf </th>
   <th style="text-align:right;"> pour.Site_transf </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:left;"> approv_combust </td>
   <td style="text-align:right;"> 3 </td>
   <td style="text-align:right;"> 3.370786 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> boutiq_mat_transf </td>
   <td style="text-align:right;"> 4 </td>
   <td style="text-align:right;"> 4.494382 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> certificat_site </td>
   <td style="text-align:right;"> 3 </td>
   <td style="text-align:right;"> 3.370786 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> couverture_internet </td>
   <td style="text-align:right;"> 7 </td>
   <td style="text-align:right;"> 7.865169 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> couverture_telephone </td>
   <td style="text-align:right;"> 10 </td>
   <td style="text-align:right;"> 11.235955 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> electricite_public </td>
   <td style="text-align:right;"> 8 </td>
   <td style="text-align:right;"> 8.988764 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> latrine_douche </td>
   <td style="text-align:right;"> 13 </td>
   <td style="text-align:right;"> 14.606742 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> magasinage_prod_site </td>
   <td style="text-align:right;"> 9 </td>
   <td style="text-align:right;"> 10.112360 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> parking </td>
   <td style="text-align:right;"> 2 </td>
   <td style="text-align:right;"> 2.247191 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> robinet </td>
   <td style="text-align:right;"> 4 </td>
   <td style="text-align:right;"> 4.494382 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> site_amenage_am </td>
   <td style="text-align:right;"> 11 </td>
   <td style="text-align:right;"> 12.359551 </td>
  </tr>
</tbody>
</table>



```r
ggplot(commodites)+
  geom_bar(stat='identity',aes(x=Commodites,y=pour.Site_transf))+
   theme(axis.text.x = element_text(angle = 90, vjust = 0.5, hjust=1))
```

<img src="/tmp/RtmpItTkgh/preview-2658e22e89d0f4.dir/Rapports_base2_files/figure-html/unnamed-chunk-38-1.png" width="672" />

### Commodités Existe mais ne  fonctionne pas bien ou pas du tout Région


```r
total.reg<- liste.sites.init %>% 
  group_by(niveau_administr_01) %>%
  dplyr::summarise(total.reg=n())


liste.sites.init[,c(1,12,14,seq(30,41))]  %>%
  pivot_longer( cols = names(liste.sites)[c(seq(30,41))], names_to = "Commodites") %>%
  filter(value=='Existe mais ne  fonctionne pas bien ou pas du tout') %>%
  group_by(niveau_administr_01,Commodites) %>%
  summarize(nb.sites=n()) %>% inner_join(total.reg) %>%
   dplyr::mutate(pour.Site_transf=100*nb.sites/total.reg)->commodites.reg
```

```
## `summarise()` has grouped output by 'niveau_administr_01'. You can override
## using the `.groups` argument.
## Joining, by = "niveau_administr_01"
```

```r
commodites.reg %>%
  kbl() %>% kable_styling()
```

<table class="table" style="margin-left: auto; margin-right: auto;">
 <thead>
  <tr>
   <th style="text-align:left;"> niveau_administr_01 </th>
   <th style="text-align:left;"> Commodites </th>
   <th style="text-align:right;"> nb.sites </th>
   <th style="text-align:right;"> total.reg </th>
   <th style="text-align:right;"> pour.Site_transf </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:left;"> Dakar </td>
   <td style="text-align:left;"> latrine_douche </td>
   <td style="text-align:right;"> 8 </td>
   <td style="text-align:right;"> 15 </td>
   <td style="text-align:right;"> 53.333333 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Dakar </td>
   <td style="text-align:left;"> site_amenage_am </td>
   <td style="text-align:right;"> 2 </td>
   <td style="text-align:right;"> 15 </td>
   <td style="text-align:right;"> 13.333333 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Fatick </td>
   <td style="text-align:left;"> boutiq_mat_transf </td>
   <td style="text-align:right;"> 2 </td>
   <td style="text-align:right;"> 29 </td>
   <td style="text-align:right;"> 6.896552 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Fatick </td>
   <td style="text-align:left;"> certificat_site </td>
   <td style="text-align:right;"> 2 </td>
   <td style="text-align:right;"> 29 </td>
   <td style="text-align:right;"> 6.896552 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Fatick </td>
   <td style="text-align:left;"> couverture_internet </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 29 </td>
   <td style="text-align:right;"> 3.448276 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Fatick </td>
   <td style="text-align:left;"> couverture_telephone </td>
   <td style="text-align:right;"> 5 </td>
   <td style="text-align:right;"> 29 </td>
   <td style="text-align:right;"> 17.241379 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Fatick </td>
   <td style="text-align:left;"> electricite_public </td>
   <td style="text-align:right;"> 2 </td>
   <td style="text-align:right;"> 29 </td>
   <td style="text-align:right;"> 6.896552 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Fatick </td>
   <td style="text-align:left;"> magasinage_prod_site </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 29 </td>
   <td style="text-align:right;"> 3.448276 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Fatick </td>
   <td style="text-align:left;"> robinet </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 29 </td>
   <td style="text-align:right;"> 3.448276 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Fatick </td>
   <td style="text-align:left;"> site_amenage_am </td>
   <td style="text-align:right;"> 5 </td>
   <td style="text-align:right;"> 29 </td>
   <td style="text-align:right;"> 17.241379 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Kaolack </td>
   <td style="text-align:left;"> magasinage_prod_site </td>
   <td style="text-align:right;"> 2 </td>
   <td style="text-align:right;"> 3 </td>
   <td style="text-align:right;"> 66.666667 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Louga </td>
   <td style="text-align:left;"> electricite_public </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 3 </td>
   <td style="text-align:right;"> 33.333333 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Louga </td>
   <td style="text-align:left;"> latrine_douche </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 3 </td>
   <td style="text-align:right;"> 33.333333 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Louga </td>
   <td style="text-align:left;"> magasinage_prod_site </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 3 </td>
   <td style="text-align:right;"> 33.333333 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Louga </td>
   <td style="text-align:left;"> robinet </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 3 </td>
   <td style="text-align:right;"> 33.333333 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Saint Louis </td>
   <td style="text-align:left;"> approv_combust </td>
   <td style="text-align:right;"> 3 </td>
   <td style="text-align:right;"> 13 </td>
   <td style="text-align:right;"> 23.076923 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Saint Louis </td>
   <td style="text-align:left;"> couverture_internet </td>
   <td style="text-align:right;"> 2 </td>
   <td style="text-align:right;"> 13 </td>
   <td style="text-align:right;"> 15.384615 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Saint Louis </td>
   <td style="text-align:left;"> couverture_telephone </td>
   <td style="text-align:right;"> 2 </td>
   <td style="text-align:right;"> 13 </td>
   <td style="text-align:right;"> 15.384615 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Saint Louis </td>
   <td style="text-align:left;"> electricite_public </td>
   <td style="text-align:right;"> 3 </td>
   <td style="text-align:right;"> 13 </td>
   <td style="text-align:right;"> 23.076923 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Thies </td>
   <td style="text-align:left;"> boutiq_mat_transf </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 17 </td>
   <td style="text-align:right;"> 5.882353 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Thies </td>
   <td style="text-align:left;"> certificat_site </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 17 </td>
   <td style="text-align:right;"> 5.882353 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Thies </td>
   <td style="text-align:left;"> couverture_internet </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 17 </td>
   <td style="text-align:right;"> 5.882353 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Thies </td>
   <td style="text-align:left;"> couverture_telephone </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 17 </td>
   <td style="text-align:right;"> 5.882353 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Thies </td>
   <td style="text-align:left;"> electricite_public </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 17 </td>
   <td style="text-align:right;"> 5.882353 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Thies </td>
   <td style="text-align:left;"> latrine_douche </td>
   <td style="text-align:right;"> 2 </td>
   <td style="text-align:right;"> 17 </td>
   <td style="text-align:right;"> 11.764706 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Thies </td>
   <td style="text-align:left;"> magasinage_prod_site </td>
   <td style="text-align:right;"> 3 </td>
   <td style="text-align:right;"> 17 </td>
   <td style="text-align:right;"> 17.647059 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Thies </td>
   <td style="text-align:left;"> parking </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 17 </td>
   <td style="text-align:right;"> 5.882353 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Thies </td>
   <td style="text-align:left;"> robinet </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 17 </td>
   <td style="text-align:right;"> 5.882353 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Thies </td>
   <td style="text-align:left;"> site_amenage_am </td>
   <td style="text-align:right;"> 3 </td>
   <td style="text-align:right;"> 17 </td>
   <td style="text-align:right;"> 17.647059 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Ziguinchor </td>
   <td style="text-align:left;"> boutiq_mat_transf </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 9 </td>
   <td style="text-align:right;"> 11.111111 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Ziguinchor </td>
   <td style="text-align:left;"> couverture_internet </td>
   <td style="text-align:right;"> 3 </td>
   <td style="text-align:right;"> 9 </td>
   <td style="text-align:right;"> 33.333333 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Ziguinchor </td>
   <td style="text-align:left;"> couverture_telephone </td>
   <td style="text-align:right;"> 2 </td>
   <td style="text-align:right;"> 9 </td>
   <td style="text-align:right;"> 22.222222 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Ziguinchor </td>
   <td style="text-align:left;"> electricite_public </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 9 </td>
   <td style="text-align:right;"> 11.111111 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Ziguinchor </td>
   <td style="text-align:left;"> latrine_douche </td>
   <td style="text-align:right;"> 2 </td>
   <td style="text-align:right;"> 9 </td>
   <td style="text-align:right;"> 22.222222 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Ziguinchor </td>
   <td style="text-align:left;"> magasinage_prod_site </td>
   <td style="text-align:right;"> 2 </td>
   <td style="text-align:right;"> 9 </td>
   <td style="text-align:right;"> 22.222222 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Ziguinchor </td>
   <td style="text-align:left;"> parking </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 9 </td>
   <td style="text-align:right;"> 11.111111 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Ziguinchor </td>
   <td style="text-align:left;"> robinet </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 9 </td>
   <td style="text-align:right;"> 11.111111 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Ziguinchor </td>
   <td style="text-align:left;"> site_amenage_am </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 9 </td>
   <td style="text-align:right;"> 11.111111 </td>
  </tr>
</tbody>
</table>



```r
ggplot(commodites.reg)+
  geom_bar(stat='identity',aes(x=Commodites,y=pour.Site_transf))+
  facet_wrap(~niveau_administr_01)+
   theme(axis.text.x = element_text(angle = 90, vjust = 0.5, hjust=1))
```

<img src="/tmp/RtmpItTkgh/preview-2658e22e89d0f4.dir/Rapports_base2_files/figure-html/unnamed-chunk-40-1.png" width="672" />


### Commodités Absent Pays


```r
total<-dim(liste.sites.init)[1]

liste.sites.init[,c(1,14,seq(30,41))]  %>%
  pivot_longer( cols = names(liste.sites)[c(seq(30,41))], names_to = "Commodites") %>%
  filter(value=='Absent') %>%
  group_by(Commodites) %>%
  summarize(Site_transf=n())%>%
 mutate(pour.Site_transf=Site_transf/total*100) -> commodites

commodites %>%
  kbl() %>% kable_styling()
```

<table class="table" style="margin-left: auto; margin-right: auto;">
 <thead>
  <tr>
   <th style="text-align:left;"> Commodites </th>
   <th style="text-align:right;"> Site_transf </th>
   <th style="text-align:right;"> pour.Site_transf </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:left;"> approv_combust </td>
   <td style="text-align:right;"> 61 </td>
   <td style="text-align:right;"> 68.53933 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> boutiq_mat_transf </td>
   <td style="text-align:right;"> 73 </td>
   <td style="text-align:right;"> 82.02247 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> certificat_site </td>
   <td style="text-align:right;"> 25 </td>
   <td style="text-align:right;"> 28.08989 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> couverture_internet </td>
   <td style="text-align:right;"> 16 </td>
   <td style="text-align:right;"> 17.97753 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> couverture_telephone </td>
   <td style="text-align:right;"> 11 </td>
   <td style="text-align:right;"> 12.35955 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> electricite_public </td>
   <td style="text-align:right;"> 43 </td>
   <td style="text-align:right;"> 48.31461 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> latrine_douche </td>
   <td style="text-align:right;"> 42 </td>
   <td style="text-align:right;"> 47.19101 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> magasinage_prod_site </td>
   <td style="text-align:right;"> 53 </td>
   <td style="text-align:right;"> 59.55056 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> parking </td>
   <td style="text-align:right;"> 77 </td>
   <td style="text-align:right;"> 86.51685 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> ramassage_traitem_dechets </td>
   <td style="text-align:right;"> 62 </td>
   <td style="text-align:right;"> 69.66292 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> robinet </td>
   <td style="text-align:right;"> 61 </td>
   <td style="text-align:right;"> 68.53933 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> site_amenage_am </td>
   <td style="text-align:right;"> 45 </td>
   <td style="text-align:right;"> 50.56180 </td>
  </tr>
</tbody>
</table>



```r
ggplot(commodites)+
  geom_bar(stat='identity',aes(x=Commodites,y=pour.Site_transf))+
   theme(axis.text.x = element_text(angle = 90, vjust = 0.5, hjust=1))
```

<img src="/tmp/RtmpItTkgh/preview-2658e22e89d0f4.dir/Rapports_base2_files/figure-html/unnamed-chunk-42-1.png" width="672" />

### Commodités Absent Région


```r
total.reg<- liste.sites.init %>% 
  group_by(niveau_administr_01) %>%
  dplyr::summarise(total.reg=n())


liste.sites.init[,c(1,12,14,seq(30,41))]  %>%
  pivot_longer( cols = names(liste.sites)[c(seq(30,41))], names_to = "Commodites") %>%
  filter(value=='Absent') %>%
  group_by(niveau_administr_01,Commodites) %>%
  summarize(nb.sites=n()) %>% inner_join(total.reg) %>%
   dplyr::mutate(pour.Site_transf=100*nb.sites/total.reg)->commodites.reg
```

```
## `summarise()` has grouped output by 'niveau_administr_01'. You can override
## using the `.groups` argument.
## Joining, by = "niveau_administr_01"
```

```r
commodites.reg %>%
  kbl() %>% kable_styling()
```

<table class="table" style="margin-left: auto; margin-right: auto;">
 <thead>
  <tr>
   <th style="text-align:left;"> niveau_administr_01 </th>
   <th style="text-align:left;"> Commodites </th>
   <th style="text-align:right;"> nb.sites </th>
   <th style="text-align:right;"> total.reg </th>
   <th style="text-align:right;"> pour.Site_transf </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:left;"> Dakar </td>
   <td style="text-align:left;"> approv_combust </td>
   <td style="text-align:right;"> 7 </td>
   <td style="text-align:right;"> 15 </td>
   <td style="text-align:right;"> 46.666667 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Dakar </td>
   <td style="text-align:left;"> boutiq_mat_transf </td>
   <td style="text-align:right;"> 8 </td>
   <td style="text-align:right;"> 15 </td>
   <td style="text-align:right;"> 53.333333 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Dakar </td>
   <td style="text-align:left;"> couverture_internet </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 15 </td>
   <td style="text-align:right;"> 6.666667 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Dakar </td>
   <td style="text-align:left;"> couverture_telephone </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 15 </td>
   <td style="text-align:right;"> 6.666667 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Dakar </td>
   <td style="text-align:left;"> latrine_douche </td>
   <td style="text-align:right;"> 3 </td>
   <td style="text-align:right;"> 15 </td>
   <td style="text-align:right;"> 20.000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Dakar </td>
   <td style="text-align:left;"> magasinage_prod_site </td>
   <td style="text-align:right;"> 5 </td>
   <td style="text-align:right;"> 15 </td>
   <td style="text-align:right;"> 33.333333 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Dakar </td>
   <td style="text-align:left;"> parking </td>
   <td style="text-align:right;"> 14 </td>
   <td style="text-align:right;"> 15 </td>
   <td style="text-align:right;"> 93.333333 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Dakar </td>
   <td style="text-align:left;"> ramassage_traitem_dechets </td>
   <td style="text-align:right;"> 2 </td>
   <td style="text-align:right;"> 15 </td>
   <td style="text-align:right;"> 13.333333 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Dakar </td>
   <td style="text-align:left;"> robinet </td>
   <td style="text-align:right;"> 11 </td>
   <td style="text-align:right;"> 15 </td>
   <td style="text-align:right;"> 73.333333 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Dakar </td>
   <td style="text-align:left;"> site_amenage_am </td>
   <td style="text-align:right;"> 4 </td>
   <td style="text-align:right;"> 15 </td>
   <td style="text-align:right;"> 26.666667 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Fatick </td>
   <td style="text-align:left;"> approv_combust </td>
   <td style="text-align:right;"> 24 </td>
   <td style="text-align:right;"> 29 </td>
   <td style="text-align:right;"> 82.758621 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Fatick </td>
   <td style="text-align:left;"> boutiq_mat_transf </td>
   <td style="text-align:right;"> 24 </td>
   <td style="text-align:right;"> 29 </td>
   <td style="text-align:right;"> 82.758621 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Fatick </td>
   <td style="text-align:left;"> certificat_site </td>
   <td style="text-align:right;"> 12 </td>
   <td style="text-align:right;"> 29 </td>
   <td style="text-align:right;"> 41.379310 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Fatick </td>
   <td style="text-align:left;"> couverture_internet </td>
   <td style="text-align:right;"> 9 </td>
   <td style="text-align:right;"> 29 </td>
   <td style="text-align:right;"> 31.034483 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Fatick </td>
   <td style="text-align:left;"> couverture_telephone </td>
   <td style="text-align:right;"> 5 </td>
   <td style="text-align:right;"> 29 </td>
   <td style="text-align:right;"> 17.241379 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Fatick </td>
   <td style="text-align:left;"> electricite_public </td>
   <td style="text-align:right;"> 19 </td>
   <td style="text-align:right;"> 29 </td>
   <td style="text-align:right;"> 65.517241 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Fatick </td>
   <td style="text-align:left;"> latrine_douche </td>
   <td style="text-align:right;"> 19 </td>
   <td style="text-align:right;"> 29 </td>
   <td style="text-align:right;"> 65.517241 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Fatick </td>
   <td style="text-align:left;"> magasinage_prod_site </td>
   <td style="text-align:right;"> 22 </td>
   <td style="text-align:right;"> 29 </td>
   <td style="text-align:right;"> 75.862069 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Fatick </td>
   <td style="text-align:left;"> parking </td>
   <td style="text-align:right;"> 25 </td>
   <td style="text-align:right;"> 29 </td>
   <td style="text-align:right;"> 86.206897 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Fatick </td>
   <td style="text-align:left;"> ramassage_traitem_dechets </td>
   <td style="text-align:right;"> 24 </td>
   <td style="text-align:right;"> 29 </td>
   <td style="text-align:right;"> 82.758621 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Fatick </td>
   <td style="text-align:left;"> robinet </td>
   <td style="text-align:right;"> 20 </td>
   <td style="text-align:right;"> 29 </td>
   <td style="text-align:right;"> 68.965517 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Fatick </td>
   <td style="text-align:left;"> site_amenage_am </td>
   <td style="text-align:right;"> 16 </td>
   <td style="text-align:right;"> 29 </td>
   <td style="text-align:right;"> 55.172414 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Kaolack </td>
   <td style="text-align:left;"> boutiq_mat_transf </td>
   <td style="text-align:right;"> 2 </td>
   <td style="text-align:right;"> 3 </td>
   <td style="text-align:right;"> 66.666667 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Kaolack </td>
   <td style="text-align:left;"> certificat_site </td>
   <td style="text-align:right;"> 3 </td>
   <td style="text-align:right;"> 3 </td>
   <td style="text-align:right;"> 100.000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Kaolack </td>
   <td style="text-align:left;"> ramassage_traitem_dechets </td>
   <td style="text-align:right;"> 3 </td>
   <td style="text-align:right;"> 3 </td>
   <td style="text-align:right;"> 100.000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Kaolack </td>
   <td style="text-align:left;"> robinet </td>
   <td style="text-align:right;"> 3 </td>
   <td style="text-align:right;"> 3 </td>
   <td style="text-align:right;"> 100.000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Louga </td>
   <td style="text-align:left;"> approv_combust </td>
   <td style="text-align:right;"> 2 </td>
   <td style="text-align:right;"> 3 </td>
   <td style="text-align:right;"> 66.666667 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Louga </td>
   <td style="text-align:left;"> boutiq_mat_transf </td>
   <td style="text-align:right;"> 3 </td>
   <td style="text-align:right;"> 3 </td>
   <td style="text-align:right;"> 100.000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Louga </td>
   <td style="text-align:left;"> electricite_public </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 3 </td>
   <td style="text-align:right;"> 33.333333 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Louga </td>
   <td style="text-align:left;"> latrine_douche </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 3 </td>
   <td style="text-align:right;"> 33.333333 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Louga </td>
   <td style="text-align:left;"> magasinage_prod_site </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 3 </td>
   <td style="text-align:right;"> 33.333333 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Louga </td>
   <td style="text-align:left;"> parking </td>
   <td style="text-align:right;"> 3 </td>
   <td style="text-align:right;"> 3 </td>
   <td style="text-align:right;"> 100.000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Louga </td>
   <td style="text-align:left;"> ramassage_traitem_dechets </td>
   <td style="text-align:right;"> 2 </td>
   <td style="text-align:right;"> 3 </td>
   <td style="text-align:right;"> 66.666667 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Louga </td>
   <td style="text-align:left;"> robinet </td>
   <td style="text-align:right;"> 2 </td>
   <td style="text-align:right;"> 3 </td>
   <td style="text-align:right;"> 66.666667 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Louga </td>
   <td style="text-align:left;"> site_amenage_am </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 3 </td>
   <td style="text-align:right;"> 33.333333 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Saint Louis </td>
   <td style="text-align:left;"> approv_combust </td>
   <td style="text-align:right;"> 9 </td>
   <td style="text-align:right;"> 13 </td>
   <td style="text-align:right;"> 69.230769 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Saint Louis </td>
   <td style="text-align:left;"> boutiq_mat_transf </td>
   <td style="text-align:right;"> 13 </td>
   <td style="text-align:right;"> 13 </td>
   <td style="text-align:right;"> 100.000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Saint Louis </td>
   <td style="text-align:left;"> certificat_site </td>
   <td style="text-align:right;"> 4 </td>
   <td style="text-align:right;"> 13 </td>
   <td style="text-align:right;"> 30.769231 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Saint Louis </td>
   <td style="text-align:left;"> couverture_internet </td>
   <td style="text-align:right;"> 2 </td>
   <td style="text-align:right;"> 13 </td>
   <td style="text-align:right;"> 15.384615 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Saint Louis </td>
   <td style="text-align:left;"> couverture_telephone </td>
   <td style="text-align:right;"> 2 </td>
   <td style="text-align:right;"> 13 </td>
   <td style="text-align:right;"> 15.384615 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Saint Louis </td>
   <td style="text-align:left;"> electricite_public </td>
   <td style="text-align:right;"> 5 </td>
   <td style="text-align:right;"> 13 </td>
   <td style="text-align:right;"> 38.461539 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Saint Louis </td>
   <td style="text-align:left;"> latrine_douche </td>
   <td style="text-align:right;"> 12 </td>
   <td style="text-align:right;"> 13 </td>
   <td style="text-align:right;"> 92.307692 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Saint Louis </td>
   <td style="text-align:left;"> magasinage_prod_site </td>
   <td style="text-align:right;"> 12 </td>
   <td style="text-align:right;"> 13 </td>
   <td style="text-align:right;"> 92.307692 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Saint Louis </td>
   <td style="text-align:left;"> parking </td>
   <td style="text-align:right;"> 12 </td>
   <td style="text-align:right;"> 13 </td>
   <td style="text-align:right;"> 92.307692 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Saint Louis </td>
   <td style="text-align:left;"> ramassage_traitem_dechets </td>
   <td style="text-align:right;"> 12 </td>
   <td style="text-align:right;"> 13 </td>
   <td style="text-align:right;"> 92.307692 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Saint Louis </td>
   <td style="text-align:left;"> robinet </td>
   <td style="text-align:right;"> 13 </td>
   <td style="text-align:right;"> 13 </td>
   <td style="text-align:right;"> 100.000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Saint Louis </td>
   <td style="text-align:left;"> site_amenage_am </td>
   <td style="text-align:right;"> 12 </td>
   <td style="text-align:right;"> 13 </td>
   <td style="text-align:right;"> 92.307692 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Thies </td>
   <td style="text-align:left;"> approv_combust </td>
   <td style="text-align:right;"> 13 </td>
   <td style="text-align:right;"> 17 </td>
   <td style="text-align:right;"> 76.470588 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Thies </td>
   <td style="text-align:left;"> boutiq_mat_transf </td>
   <td style="text-align:right;"> 16 </td>
   <td style="text-align:right;"> 17 </td>
   <td style="text-align:right;"> 94.117647 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Thies </td>
   <td style="text-align:left;"> certificat_site </td>
   <td style="text-align:right;"> 4 </td>
   <td style="text-align:right;"> 17 </td>
   <td style="text-align:right;"> 23.529412 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Thies </td>
   <td style="text-align:left;"> couverture_internet </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 17 </td>
   <td style="text-align:right;"> 5.882353 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Thies </td>
   <td style="text-align:left;"> couverture_telephone </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 17 </td>
   <td style="text-align:right;"> 5.882353 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Thies </td>
   <td style="text-align:left;"> electricite_public </td>
   <td style="text-align:right;"> 11 </td>
   <td style="text-align:right;"> 17 </td>
   <td style="text-align:right;"> 64.705882 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Thies </td>
   <td style="text-align:left;"> latrine_douche </td>
   <td style="text-align:right;"> 3 </td>
   <td style="text-align:right;"> 17 </td>
   <td style="text-align:right;"> 17.647059 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Thies </td>
   <td style="text-align:left;"> magasinage_prod_site </td>
   <td style="text-align:right;"> 8 </td>
   <td style="text-align:right;"> 17 </td>
   <td style="text-align:right;"> 47.058824 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Thies </td>
   <td style="text-align:left;"> parking </td>
   <td style="text-align:right;"> 15 </td>
   <td style="text-align:right;"> 17 </td>
   <td style="text-align:right;"> 88.235294 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Thies </td>
   <td style="text-align:left;"> ramassage_traitem_dechets </td>
   <td style="text-align:right;"> 11 </td>
   <td style="text-align:right;"> 17 </td>
   <td style="text-align:right;"> 64.705882 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Thies </td>
   <td style="text-align:left;"> robinet </td>
   <td style="text-align:right;"> 7 </td>
   <td style="text-align:right;"> 17 </td>
   <td style="text-align:right;"> 41.176471 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Thies </td>
   <td style="text-align:left;"> site_amenage_am </td>
   <td style="text-align:right;"> 7 </td>
   <td style="text-align:right;"> 17 </td>
   <td style="text-align:right;"> 41.176471 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Ziguinchor </td>
   <td style="text-align:left;"> approv_combust </td>
   <td style="text-align:right;"> 6 </td>
   <td style="text-align:right;"> 9 </td>
   <td style="text-align:right;"> 66.666667 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Ziguinchor </td>
   <td style="text-align:left;"> boutiq_mat_transf </td>
   <td style="text-align:right;"> 7 </td>
   <td style="text-align:right;"> 9 </td>
   <td style="text-align:right;"> 77.777778 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Ziguinchor </td>
   <td style="text-align:left;"> certificat_site </td>
   <td style="text-align:right;"> 2 </td>
   <td style="text-align:right;"> 9 </td>
   <td style="text-align:right;"> 22.222222 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Ziguinchor </td>
   <td style="text-align:left;"> couverture_internet </td>
   <td style="text-align:right;"> 3 </td>
   <td style="text-align:right;"> 9 </td>
   <td style="text-align:right;"> 33.333333 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Ziguinchor </td>
   <td style="text-align:left;"> couverture_telephone </td>
   <td style="text-align:right;"> 2 </td>
   <td style="text-align:right;"> 9 </td>
   <td style="text-align:right;"> 22.222222 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Ziguinchor </td>
   <td style="text-align:left;"> electricite_public </td>
   <td style="text-align:right;"> 7 </td>
   <td style="text-align:right;"> 9 </td>
   <td style="text-align:right;"> 77.777778 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Ziguinchor </td>
   <td style="text-align:left;"> latrine_douche </td>
   <td style="text-align:right;"> 4 </td>
   <td style="text-align:right;"> 9 </td>
   <td style="text-align:right;"> 44.444444 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Ziguinchor </td>
   <td style="text-align:left;"> magasinage_prod_site </td>
   <td style="text-align:right;"> 5 </td>
   <td style="text-align:right;"> 9 </td>
   <td style="text-align:right;"> 55.555556 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Ziguinchor </td>
   <td style="text-align:left;"> parking </td>
   <td style="text-align:right;"> 8 </td>
   <td style="text-align:right;"> 9 </td>
   <td style="text-align:right;"> 88.888889 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Ziguinchor </td>
   <td style="text-align:left;"> ramassage_traitem_dechets </td>
   <td style="text-align:right;"> 8 </td>
   <td style="text-align:right;"> 9 </td>
   <td style="text-align:right;"> 88.888889 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Ziguinchor </td>
   <td style="text-align:left;"> robinet </td>
   <td style="text-align:right;"> 5 </td>
   <td style="text-align:right;"> 9 </td>
   <td style="text-align:right;"> 55.555556 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Ziguinchor </td>
   <td style="text-align:left;"> site_amenage_am </td>
   <td style="text-align:right;"> 5 </td>
   <td style="text-align:right;"> 9 </td>
   <td style="text-align:right;"> 55.555556 </td>
  </tr>
</tbody>
</table>



```r
ggplot(commodites.reg)+
  geom_bar(stat='identity',aes(x=Commodites,y=pour.Site_transf))+
  facet_wrap(~niveau_administr_01)+
   theme(axis.text.x = element_text(angle = 90, vjust = 0.5, hjust=1))
```

<img src="/tmp/RtmpItTkgh/preview-2658e22e89d0f4.dir/Rapports_base2_files/figure-html/unnamed-chunk-44-1.png" width="672" />



### Commodités NSP Pays


```r
total<-dim(liste.sites.init)[1]

liste.sites.init[,c(1,14,seq(30,41))]  %>%
  pivot_longer( cols = names(liste.sites)[c(seq(30,41))], names_to = "Commodites") %>%
  filter(value=='NSP') %>%
  group_by(Commodites) %>%
  summarize(Site_transf=n())%>%
 mutate(pour.Site_transf=Site_transf/total*100) -> commodites

commodites %>%
  kbl() %>% kable_styling()
```

<table class="table" style="margin-left: auto; margin-right: auto;">
 <thead>
  <tr>
   <th style="text-align:left;"> Commodites </th>
   <th style="text-align:right;"> Site_transf </th>
   <th style="text-align:right;"> pour.Site_transf </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:left;"> boutiq_mat_transf </td>
   <td style="text-align:right;"> 4 </td>
   <td style="text-align:right;"> 4.494382 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> certificat_site </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 1.123595 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> couverture_internet </td>
   <td style="text-align:right;"> 3 </td>
   <td style="text-align:right;"> 3.370786 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> couverture_telephone </td>
   <td style="text-align:right;"> 3 </td>
   <td style="text-align:right;"> 3.370786 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> electricite_public </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 1.123595 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> ramassage_traitem_dechets </td>
   <td style="text-align:right;"> 2 </td>
   <td style="text-align:right;"> 2.247191 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> robinet </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 1.123595 </td>
  </tr>
</tbody>
</table>



```r
ggplot(commodites)+
  geom_bar(stat='identity',aes(x=Commodites,y=pour.Site_transf))+
   theme(axis.text.x = element_text(angle = 90, vjust = 0.5, hjust=1))
```

<img src="/tmp/RtmpItTkgh/preview-2658e22e89d0f4.dir/Rapports_base2_files/figure-html/unnamed-chunk-46-1.png" width="672" />

### Commodités NSP Région


```r
total.reg<- liste.sites.init %>% 
  group_by(niveau_administr_01) %>%
  dplyr::summarise(total.reg=n())


liste.sites.init[,c(1,12,14,seq(30,41))]  %>%
  pivot_longer( cols = names(liste.sites)[c(seq(30,41))], names_to = "Commodites") %>%
  filter(value=='NSP') %>%
  group_by(niveau_administr_01,Commodites) %>%
  summarize(nb.sites=n()) %>% inner_join(total.reg) %>%
   dplyr::mutate(pour.Site_transf=100*nb.sites/total.reg)->commodites.reg
```

```
## `summarise()` has grouped output by 'niveau_administr_01'. You can override
## using the `.groups` argument.
## Joining, by = "niveau_administr_01"
```

```r
commodites.reg %>%
  kbl() %>% kable_styling()
```

<table class="table" style="margin-left: auto; margin-right: auto;">
 <thead>
  <tr>
   <th style="text-align:left;"> niveau_administr_01 </th>
   <th style="text-align:left;"> Commodites </th>
   <th style="text-align:right;"> nb.sites </th>
   <th style="text-align:right;"> total.reg </th>
   <th style="text-align:right;"> pour.Site_transf </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:left;"> Dakar </td>
   <td style="text-align:left;"> boutiq_mat_transf </td>
   <td style="text-align:right;"> 4 </td>
   <td style="text-align:right;"> 15 </td>
   <td style="text-align:right;"> 26.666667 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Fatick </td>
   <td style="text-align:left;"> couverture_internet </td>
   <td style="text-align:right;"> 2 </td>
   <td style="text-align:right;"> 29 </td>
   <td style="text-align:right;"> 6.896552 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Fatick </td>
   <td style="text-align:left;"> couverture_telephone </td>
   <td style="text-align:right;"> 2 </td>
   <td style="text-align:right;"> 29 </td>
   <td style="text-align:right;"> 6.896552 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Fatick </td>
   <td style="text-align:left;"> electricite_public </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 29 </td>
   <td style="text-align:right;"> 3.448276 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Fatick </td>
   <td style="text-align:left;"> ramassage_traitem_dechets </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 29 </td>
   <td style="text-align:right;"> 3.448276 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Fatick </td>
   <td style="text-align:left;"> robinet </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 29 </td>
   <td style="text-align:right;"> 3.448276 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Saint Louis </td>
   <td style="text-align:left;"> couverture_internet </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 13 </td>
   <td style="text-align:right;"> 7.692308 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Saint Louis </td>
   <td style="text-align:left;"> couverture_telephone </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 13 </td>
   <td style="text-align:right;"> 7.692308 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Saint Louis </td>
   <td style="text-align:left;"> ramassage_traitem_dechets </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 13 </td>
   <td style="text-align:right;"> 7.692308 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Ziguinchor </td>
   <td style="text-align:left;"> certificat_site </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 9 </td>
   <td style="text-align:right;"> 11.111111 </td>
  </tr>
</tbody>
</table>



```r
ggplot(commodites.reg)+
  geom_bar(stat='identity',aes(x=Commodites,y=pour.Site_transf))+
  facet_wrap(~niveau_administr_01)+
   theme(axis.text.x = element_text(angle = 90, vjust = 0.5, hjust=1))
```

<img src="/tmp/RtmpItTkgh/preview-2658e22e89d0f4.dir/Rapports_base2_files/figure-html/unnamed-chunk-48-1.png" width="672" />







### professionnels du transformé ayant accès aux commodités (Présent et fonctionne) Pays


```r
#total<-dim(liste.sites.init)[1]
total<-sum(liste.sites.init[,47], na.rm = TRUE)

liste.sites.init[,c(1,14,seq(30,41),47)]  %>%
  pivot_longer( cols = names(liste.sites)[c(seq(30,41))], names_to = "Commodites") %>%
  filter(value=='Présent et fonctionne') %>%
 # filter(value=='Existe mais ne  fonctionne pas bien ou pas du tout') %>%
 # filter(value=='Présent et fonctionne' | value=='Existe mais ne  fonctionne pas bien ou pas du tout') %>%
  group_by(Commodites) %>%
  summarize(Effectif_Operat=sum(combien_operateur_activite, na.rm = TRUE))%>%
#  donnees <- liste.sites.init(liste.sites.init[,c(seq(30,41))], effectif)
   mutate(pour.Effectif_Operat=Effectif_Operat/total*100) -> commodites

commodites %>%
  kbl() %>% kable_styling()
```

<table class="table" style="margin-left: auto; margin-right: auto;">
 <thead>
  <tr>
   <th style="text-align:left;"> Commodites </th>
   <th style="text-align:right;"> Effectif_Operat </th>
   <th style="text-align:right;"> pour.Effectif_Operat </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:left;"> approv_combust </td>
   <td style="text-align:right;"> 2642 </td>
   <td style="text-align:right;"> 54.395718 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> boutiq_mat_transf </td>
   <td style="text-align:right;"> 2190 </td>
   <td style="text-align:right;"> 45.089562 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> certificat_site </td>
   <td style="text-align:right;"> 2479 </td>
   <td style="text-align:right;"> 51.039736 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> couverture_internet </td>
   <td style="text-align:right;"> 2651 </td>
   <td style="text-align:right;"> 54.581017 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> couverture_telephone </td>
   <td style="text-align:right;"> 2735 </td>
   <td style="text-align:right;"> 56.310480 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> electricite_public </td>
   <td style="text-align:right;"> 858 </td>
   <td style="text-align:right;"> 17.665225 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> latrine_douche </td>
   <td style="text-align:right;"> 3698 </td>
   <td style="text-align:right;"> 76.137534 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> magasinage_prod_site </td>
   <td style="text-align:right;"> 2684 </td>
   <td style="text-align:right;"> 55.260449 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> parking </td>
   <td style="text-align:right;"> 76 </td>
   <td style="text-align:right;"> 1.564752 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> ramassage_traitem_dechets </td>
   <td style="text-align:right;"> 855 </td>
   <td style="text-align:right;"> 17.603459 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> robinet </td>
   <td style="text-align:right;"> 3491 </td>
   <td style="text-align:right;"> 71.875643 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> site_amenage_am </td>
   <td style="text-align:right;"> 3010 </td>
   <td style="text-align:right;"> 61.972411 </td>
  </tr>
</tbody>
</table>



```r
ggplot(commodites)+
  geom_bar(stat='identity',aes(x=Commodites,y=pour.Effectif_Operat))+
   theme(axis.text.x = element_text(angle = 90, vjust = 0.5, hjust=1))
```

<img src="/tmp/RtmpItTkgh/preview-2658e22e89d0f4.dir/Rapports_base2_files/figure-html/unnamed-chunk-50-1.png" width="672" />

### professionnels du transformé ayant accès aux commodités (Présent et fonctionne) Région


```r
total.reg<- liste.sites.init %>% 
#total.reg <-sum(liste.sites.init[,47], na.rm = TRUE)
 group_by(niveau_administr_01) %>%
 dplyr:: summarize(total.reg=sum(combien_operateur_activite, na.rm = TRUE))


liste.sites.init[,c(1,12,14,seq(30,41),47)]  %>%
  pivot_longer( cols = names(liste.sites)[c(seq(30,41))], names_to = "Commodites") %>%
  filter(value=='Présent et fonctionne') %>%
  group_by(niveau_administr_01,Commodites) %>%
  summarize(Effectif_Operat=sum(combien_operateur_activite, na.rm = TRUE)) %>% 
  
  inner_join(total.reg) %>%
   dplyr::mutate(pour.Effectif_Operat=100*Effectif_Operat/total.reg)->commodites.reg
```

```
## `summarise()` has grouped output by 'niveau_administr_01'. You can override
## using the `.groups` argument.
## Joining, by = "niveau_administr_01"
```

```r
commodites.reg %>%
  kbl() %>% kable_styling()
```

<table class="table" style="margin-left: auto; margin-right: auto;">
 <thead>
  <tr>
   <th style="text-align:left;"> niveau_administr_01 </th>
   <th style="text-align:left;"> Commodites </th>
   <th style="text-align:right;"> Effectif_Operat </th>
   <th style="text-align:right;"> total.reg </th>
   <th style="text-align:right;"> pour.Effectif_Operat </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:left;"> Dakar </td>
   <td style="text-align:left;"> approv_combust </td>
   <td style="text-align:right;"> 74 </td>
   <td style="text-align:right;"> 359 </td>
   <td style="text-align:right;"> 20.612813 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Dakar </td>
   <td style="text-align:left;"> boutiq_mat_transf </td>
   <td style="text-align:right;"> 190 </td>
   <td style="text-align:right;"> 359 </td>
   <td style="text-align:right;"> 52.924791 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Dakar </td>
   <td style="text-align:left;"> certificat_site </td>
   <td style="text-align:right;"> 329 </td>
   <td style="text-align:right;"> 359 </td>
   <td style="text-align:right;"> 91.643454 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Dakar </td>
   <td style="text-align:left;"> couverture_internet </td>
   <td style="text-align:right;"> 357 </td>
   <td style="text-align:right;"> 359 </td>
   <td style="text-align:right;"> 99.442897 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Dakar </td>
   <td style="text-align:left;"> couverture_telephone </td>
   <td style="text-align:right;"> 357 </td>
   <td style="text-align:right;"> 359 </td>
   <td style="text-align:right;"> 99.442897 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Dakar </td>
   <td style="text-align:left;"> electricite_public </td>
   <td style="text-align:right;"> 359 </td>
   <td style="text-align:right;"> 359 </td>
   <td style="text-align:right;"> 100.000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Dakar </td>
   <td style="text-align:left;"> latrine_douche </td>
   <td style="text-align:right;"> 74 </td>
   <td style="text-align:right;"> 359 </td>
   <td style="text-align:right;"> 20.612813 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Dakar </td>
   <td style="text-align:left;"> magasinage_prod_site </td>
   <td style="text-align:right;"> 129 </td>
   <td style="text-align:right;"> 359 </td>
   <td style="text-align:right;"> 35.933148 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Dakar </td>
   <td style="text-align:left;"> parking </td>
   <td style="text-align:right;"> 25 </td>
   <td style="text-align:right;"> 359 </td>
   <td style="text-align:right;"> 6.963788 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Dakar </td>
   <td style="text-align:left;"> ramassage_traitem_dechets </td>
   <td style="text-align:right;"> 357 </td>
   <td style="text-align:right;"> 359 </td>
   <td style="text-align:right;"> 99.442897 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Dakar </td>
   <td style="text-align:left;"> robinet </td>
   <td style="text-align:right;"> 87 </td>
   <td style="text-align:right;"> 359 </td>
   <td style="text-align:right;"> 24.233983 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Dakar </td>
   <td style="text-align:left;"> site_amenage_am </td>
   <td style="text-align:right;"> 127 </td>
   <td style="text-align:right;"> 359 </td>
   <td style="text-align:right;"> 35.376045 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Fatick </td>
   <td style="text-align:left;"> approv_combust </td>
   <td style="text-align:right;"> 96 </td>
   <td style="text-align:right;"> 351 </td>
   <td style="text-align:right;"> 27.350427 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Fatick </td>
   <td style="text-align:left;"> boutiq_mat_transf </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 351 </td>
   <td style="text-align:right;"> 0.000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Fatick </td>
   <td style="text-align:left;"> certificat_site </td>
   <td style="text-align:right;"> 153 </td>
   <td style="text-align:right;"> 351 </td>
   <td style="text-align:right;"> 43.589744 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Fatick </td>
   <td style="text-align:left;"> couverture_internet </td>
   <td style="text-align:right;"> 160 </td>
   <td style="text-align:right;"> 351 </td>
   <td style="text-align:right;"> 45.584046 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Fatick </td>
   <td style="text-align:left;"> couverture_telephone </td>
   <td style="text-align:right;"> 232 </td>
   <td style="text-align:right;"> 351 </td>
   <td style="text-align:right;"> 66.096866 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Fatick </td>
   <td style="text-align:left;"> electricite_public </td>
   <td style="text-align:right;"> 109 </td>
   <td style="text-align:right;"> 351 </td>
   <td style="text-align:right;"> 31.054131 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Fatick </td>
   <td style="text-align:left;"> latrine_douche </td>
   <td style="text-align:right;"> 189 </td>
   <td style="text-align:right;"> 351 </td>
   <td style="text-align:right;"> 53.846154 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Fatick </td>
   <td style="text-align:left;"> magasinage_prod_site </td>
   <td style="text-align:right;"> 160 </td>
   <td style="text-align:right;"> 351 </td>
   <td style="text-align:right;"> 45.584046 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Fatick </td>
   <td style="text-align:left;"> parking </td>
   <td style="text-align:right;"> 16 </td>
   <td style="text-align:right;"> 351 </td>
   <td style="text-align:right;"> 4.558405 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Fatick </td>
   <td style="text-align:left;"> ramassage_traitem_dechets </td>
   <td style="text-align:right;"> 144 </td>
   <td style="text-align:right;"> 351 </td>
   <td style="text-align:right;"> 41.025641 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Fatick </td>
   <td style="text-align:left;"> robinet </td>
   <td style="text-align:right;"> 189 </td>
   <td style="text-align:right;"> 351 </td>
   <td style="text-align:right;"> 53.846154 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Fatick </td>
   <td style="text-align:left;"> site_amenage_am </td>
   <td style="text-align:right;"> 173 </td>
   <td style="text-align:right;"> 351 </td>
   <td style="text-align:right;"> 49.287749 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Kaolack </td>
   <td style="text-align:left;"> approv_combust </td>
   <td style="text-align:right;"> 10 </td>
   <td style="text-align:right;"> 10 </td>
   <td style="text-align:right;"> 100.000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Kaolack </td>
   <td style="text-align:left;"> boutiq_mat_transf </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 10 </td>
   <td style="text-align:right;"> 0.000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Kaolack </td>
   <td style="text-align:left;"> couverture_internet </td>
   <td style="text-align:right;"> 10 </td>
   <td style="text-align:right;"> 10 </td>
   <td style="text-align:right;"> 100.000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Kaolack </td>
   <td style="text-align:left;"> couverture_telephone </td>
   <td style="text-align:right;"> 10 </td>
   <td style="text-align:right;"> 10 </td>
   <td style="text-align:right;"> 100.000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Kaolack </td>
   <td style="text-align:left;"> electricite_public </td>
   <td style="text-align:right;"> 10 </td>
   <td style="text-align:right;"> 10 </td>
   <td style="text-align:right;"> 100.000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Kaolack </td>
   <td style="text-align:left;"> latrine_douche </td>
   <td style="text-align:right;"> 10 </td>
   <td style="text-align:right;"> 10 </td>
   <td style="text-align:right;"> 100.000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Kaolack </td>
   <td style="text-align:left;"> magasinage_prod_site </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 10 </td>
   <td style="text-align:right;"> 0.000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Kaolack </td>
   <td style="text-align:left;"> parking </td>
   <td style="text-align:right;"> 10 </td>
   <td style="text-align:right;"> 10 </td>
   <td style="text-align:right;"> 100.000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Kaolack </td>
   <td style="text-align:left;"> site_amenage_am </td>
   <td style="text-align:right;"> 10 </td>
   <td style="text-align:right;"> 10 </td>
   <td style="text-align:right;"> 100.000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Louga </td>
   <td style="text-align:left;"> approv_combust </td>
   <td style="text-align:right;"> 160 </td>
   <td style="text-align:right;"> 200 </td>
   <td style="text-align:right;"> 80.000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Louga </td>
   <td style="text-align:left;"> certificat_site </td>
   <td style="text-align:right;"> 200 </td>
   <td style="text-align:right;"> 200 </td>
   <td style="text-align:right;"> 100.000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Louga </td>
   <td style="text-align:left;"> couverture_internet </td>
   <td style="text-align:right;"> 200 </td>
   <td style="text-align:right;"> 200 </td>
   <td style="text-align:right;"> 100.000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Louga </td>
   <td style="text-align:left;"> couverture_telephone </td>
   <td style="text-align:right;"> 200 </td>
   <td style="text-align:right;"> 200 </td>
   <td style="text-align:right;"> 100.000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Louga </td>
   <td style="text-align:left;"> electricite_public </td>
   <td style="text-align:right;"> 160 </td>
   <td style="text-align:right;"> 200 </td>
   <td style="text-align:right;"> 80.000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Louga </td>
   <td style="text-align:left;"> latrine_douche </td>
   <td style="text-align:right;"> 160 </td>
   <td style="text-align:right;"> 200 </td>
   <td style="text-align:right;"> 80.000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Louga </td>
   <td style="text-align:left;"> magasinage_prod_site </td>
   <td style="text-align:right;"> 160 </td>
   <td style="text-align:right;"> 200 </td>
   <td style="text-align:right;"> 80.000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Louga </td>
   <td style="text-align:left;"> ramassage_traitem_dechets </td>
   <td style="text-align:right;"> 160 </td>
   <td style="text-align:right;"> 200 </td>
   <td style="text-align:right;"> 80.000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Louga </td>
   <td style="text-align:left;"> site_amenage_am </td>
   <td style="text-align:right;"> 165 </td>
   <td style="text-align:right;"> 200 </td>
   <td style="text-align:right;"> 82.500000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Saint Louis </td>
   <td style="text-align:left;"> approv_combust </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 361 </td>
   <td style="text-align:right;"> 0.000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Saint Louis </td>
   <td style="text-align:left;"> certificat_site </td>
   <td style="text-align:right;"> 360 </td>
   <td style="text-align:right;"> 361 </td>
   <td style="text-align:right;"> 99.722992 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Saint Louis </td>
   <td style="text-align:left;"> couverture_internet </td>
   <td style="text-align:right;"> 360 </td>
   <td style="text-align:right;"> 361 </td>
   <td style="text-align:right;"> 99.722992 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Saint Louis </td>
   <td style="text-align:left;"> couverture_telephone </td>
   <td style="text-align:right;"> 360 </td>
   <td style="text-align:right;"> 361 </td>
   <td style="text-align:right;"> 99.722992 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Saint Louis </td>
   <td style="text-align:left;"> electricite_public </td>
   <td style="text-align:right;"> 60 </td>
   <td style="text-align:right;"> 361 </td>
   <td style="text-align:right;"> 16.620499 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Saint Louis </td>
   <td style="text-align:left;"> latrine_douche </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 361 </td>
   <td style="text-align:right;"> 0.000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Saint Louis </td>
   <td style="text-align:left;"> magasinage_prod_site </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 361 </td>
   <td style="text-align:right;"> 0.000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Saint Louis </td>
   <td style="text-align:left;"> parking </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 361 </td>
   <td style="text-align:right;"> 0.000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Saint Louis </td>
   <td style="text-align:left;"> site_amenage_am </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 361 </td>
   <td style="text-align:right;"> 0.000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Thies </td>
   <td style="text-align:left;"> approv_combust </td>
   <td style="text-align:right;"> 290 </td>
   <td style="text-align:right;"> 1434 </td>
   <td style="text-align:right;"> 20.223152 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Thies </td>
   <td style="text-align:left;"> certificat_site </td>
   <td style="text-align:right;"> 1295 </td>
   <td style="text-align:right;"> 1434 </td>
   <td style="text-align:right;"> 90.306834 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Thies </td>
   <td style="text-align:left;"> couverture_internet </td>
   <td style="text-align:right;"> 1434 </td>
   <td style="text-align:right;"> 1434 </td>
   <td style="text-align:right;"> 100.000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Thies </td>
   <td style="text-align:left;"> couverture_telephone </td>
   <td style="text-align:right;"> 1434 </td>
   <td style="text-align:right;"> 1434 </td>
   <td style="text-align:right;"> 100.000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Thies </td>
   <td style="text-align:left;"> electricite_public </td>
   <td style="text-align:right;"> 160 </td>
   <td style="text-align:right;"> 1434 </td>
   <td style="text-align:right;"> 11.157601 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Thies </td>
   <td style="text-align:left;"> latrine_douche </td>
   <td style="text-align:right;"> 1253 </td>
   <td style="text-align:right;"> 1434 </td>
   <td style="text-align:right;"> 87.377964 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Thies </td>
   <td style="text-align:left;"> magasinage_prod_site </td>
   <td style="text-align:right;"> 223 </td>
   <td style="text-align:right;"> 1434 </td>
   <td style="text-align:right;"> 15.550907 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Thies </td>
   <td style="text-align:left;"> parking </td>
   <td style="text-align:right;"> 25 </td>
   <td style="text-align:right;"> 1434 </td>
   <td style="text-align:right;"> 1.743375 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Thies </td>
   <td style="text-align:left;"> ramassage_traitem_dechets </td>
   <td style="text-align:right;"> 194 </td>
   <td style="text-align:right;"> 1434 </td>
   <td style="text-align:right;"> 13.528591 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Thies </td>
   <td style="text-align:left;"> robinet </td>
   <td style="text-align:right;"> 1203 </td>
   <td style="text-align:right;"> 1434 </td>
   <td style="text-align:right;"> 83.891213 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Thies </td>
   <td style="text-align:left;"> site_amenage_am </td>
   <td style="text-align:right;"> 523 </td>
   <td style="text-align:right;"> 1434 </td>
   <td style="text-align:right;"> 36.471409 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Ziguinchor </td>
   <td style="text-align:left;"> approv_combust </td>
   <td style="text-align:right;"> 2012 </td>
   <td style="text-align:right;"> 2142 </td>
   <td style="text-align:right;"> 93.930906 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Ziguinchor </td>
   <td style="text-align:left;"> boutiq_mat_transf </td>
   <td style="text-align:right;"> 2000 </td>
   <td style="text-align:right;"> 2142 </td>
   <td style="text-align:right;"> 93.370682 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Ziguinchor </td>
   <td style="text-align:left;"> certificat_site </td>
   <td style="text-align:right;"> 142 </td>
   <td style="text-align:right;"> 2142 </td>
   <td style="text-align:right;"> 6.629318 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Ziguinchor </td>
   <td style="text-align:left;"> couverture_internet </td>
   <td style="text-align:right;"> 130 </td>
   <td style="text-align:right;"> 2142 </td>
   <td style="text-align:right;"> 6.069094 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Ziguinchor </td>
   <td style="text-align:left;"> couverture_telephone </td>
   <td style="text-align:right;"> 142 </td>
   <td style="text-align:right;"> 2142 </td>
   <td style="text-align:right;"> 6.629318 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Ziguinchor </td>
   <td style="text-align:left;"> electricite_public </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 2142 </td>
   <td style="text-align:right;"> 0.000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Ziguinchor </td>
   <td style="text-align:left;"> latrine_douche </td>
   <td style="text-align:right;"> 2012 </td>
   <td style="text-align:right;"> 2142 </td>
   <td style="text-align:right;"> 93.930906 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Ziguinchor </td>
   <td style="text-align:left;"> magasinage_prod_site </td>
   <td style="text-align:right;"> 2012 </td>
   <td style="text-align:right;"> 2142 </td>
   <td style="text-align:right;"> 93.930906 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Ziguinchor </td>
   <td style="text-align:left;"> ramassage_traitem_dechets </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 2142 </td>
   <td style="text-align:right;"> 0.000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Ziguinchor </td>
   <td style="text-align:left;"> robinet </td>
   <td style="text-align:right;"> 2012 </td>
   <td style="text-align:right;"> 2142 </td>
   <td style="text-align:right;"> 93.930906 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Ziguinchor </td>
   <td style="text-align:left;"> site_amenage_am </td>
   <td style="text-align:right;"> 2012 </td>
   <td style="text-align:right;"> 2142 </td>
   <td style="text-align:right;"> 93.930906 </td>
  </tr>
</tbody>
</table>



```r
ggplot(commodites.reg)+
  geom_bar(stat='identity',aes(x=Commodites,y=pour.Effectif_Operat))+
  facet_wrap(~niveau_administr_01)+
   theme(axis.text.x = element_text(angle = 90, vjust = 0.5, hjust=1))
```

<img src="/tmp/RtmpItTkgh/preview-2658e22e89d0f4.dir/Rapports_base2_files/figure-html/unnamed-chunk-52-1.png" width="672" />


### professionnels du transformé ayant accès aux commodités (Existe mais ne  fonctionne pas bien ou pas du tout) Pays


```r
#total<-dim(liste.sites.init)[1]
total<-sum(liste.sites.init[,47], na.rm = TRUE)

liste.sites.init[,c(1,14,seq(30,41),47)]  %>%
  pivot_longer( cols = names(liste.sites)[c(seq(30,41))], names_to = "Commodites") %>%
  filter(value=='Existe mais ne  fonctionne pas bien ou pas du tout') %>%
 # filter(value=='Existe mais ne  fonctionne pas bien ou pas du tout') %>%
 # filter(value=='Présent et fonctionne' | value=='Existe mais ne  fonctionne pas bien ou pas du tout') %>%
  group_by(Commodites) %>%
  summarize(Effectif_Operat=sum(combien_operateur_activite, na.rm = TRUE))%>%
#  donnees <- liste.sites.init(liste.sites.init[,c(seq(30,41))], effectif)
  
  
 mutate(pour.Effectif_Operat=Effectif_Operat/total*100) -> commodites

commodites %>%
  kbl() %>% kable_styling()
```

<table class="table" style="margin-left: auto; margin-right: auto;">
 <thead>
  <tr>
   <th style="text-align:left;"> Commodites </th>
   <th style="text-align:right;"> Effectif_Operat </th>
   <th style="text-align:right;"> pour.Effectif_Operat </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:left;"> approv_combust </td>
   <td style="text-align:right;"> 300 </td>
   <td style="text-align:right;"> 6.1766523 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> boutiq_mat_transf </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> certificat_site </td>
   <td style="text-align:right;"> 88 </td>
   <td style="text-align:right;"> 1.8118180 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> couverture_internet </td>
   <td style="text-align:right;"> 20 </td>
   <td style="text-align:right;"> 0.4117768 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> couverture_telephone </td>
   <td style="text-align:right;"> 101 </td>
   <td style="text-align:right;"> 2.0794729 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> electricite_public </td>
   <td style="text-align:right;"> 105 </td>
   <td style="text-align:right;"> 2.1618283 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> latrine_douche </td>
   <td style="text-align:right;"> 113 </td>
   <td style="text-align:right;"> 2.3265390 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> magasinage_prod_site </td>
   <td style="text-align:right;"> 145 </td>
   <td style="text-align:right;"> 2.9853819 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> parking </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> robinet </td>
   <td style="text-align:right;"> 175 </td>
   <td style="text-align:right;"> 3.6030471 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> site_amenage_am </td>
   <td style="text-align:right;"> 113 </td>
   <td style="text-align:right;"> 2.3265390 </td>
  </tr>
</tbody>
</table>



```r
ggplot(commodites)+
  geom_bar(stat='identity',aes(x=Commodites,y=pour.Effectif_Operat))+
   theme(axis.text.x = element_text(angle = 90, vjust = 0.5, hjust=1))
```

<img src="/tmp/RtmpItTkgh/preview-2658e22e89d0f4.dir/Rapports_base2_files/figure-html/unnamed-chunk-54-1.png" width="672" />

### professionnels du transformé ayant accès aux commodités (Existe mais ne  fonctionne pas bien ou pas du tout) Région


```r
total.reg<- liste.sites.init %>% 
#total.reg <-sum(liste.sites.init[,47], na.rm = TRUE)
 group_by(niveau_administr_01) %>%
 dplyr:: summarize(total.reg=sum(combien_operateur_activite, na.rm = TRUE))


liste.sites.init[,c(1,12,14,seq(30,41),47)]  %>%
  pivot_longer( cols = names(liste.sites)[c(seq(30,41))], names_to = "Commodites") %>%
  filter(value=='Existe mais ne  fonctionne pas bien ou pas du tout') %>%
  group_by(niveau_administr_01,Commodites) %>%
  summarize(Effectif_Operat=sum(combien_operateur_activite, na.rm = TRUE)) %>% 
  
  inner_join(total.reg) %>%
   dplyr::mutate(pour.Effectif_Operat=100*Effectif_Operat/total.reg)->commodites.reg
```

```
## `summarise()` has grouped output by 'niveau_administr_01'. You can override
## using the `.groups` argument.
## Joining, by = "niveau_administr_01"
```

```r
commodites.reg %>%
  kbl() %>% kable_styling()
```

<table class="table" style="margin-left: auto; margin-right: auto;">
 <thead>
  <tr>
   <th style="text-align:left;"> niveau_administr_01 </th>
   <th style="text-align:left;"> Commodites </th>
   <th style="text-align:right;"> Effectif_Operat </th>
   <th style="text-align:right;"> total.reg </th>
   <th style="text-align:right;"> pour.Effectif_Operat </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:left;"> Dakar </td>
   <td style="text-align:left;"> latrine_douche </td>
   <td style="text-align:right;"> 93 </td>
   <td style="text-align:right;"> 359 </td>
   <td style="text-align:right;"> 25.905293 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Dakar </td>
   <td style="text-align:left;"> site_amenage_am </td>
   <td style="text-align:right;"> 32 </td>
   <td style="text-align:right;"> 359 </td>
   <td style="text-align:right;"> 8.913649 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Fatick </td>
   <td style="text-align:left;"> boutiq_mat_transf </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 351 </td>
   <td style="text-align:right;"> 0.000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Fatick </td>
   <td style="text-align:left;"> certificat_site </td>
   <td style="text-align:right;"> 88 </td>
   <td style="text-align:right;"> 351 </td>
   <td style="text-align:right;"> 25.071225 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Fatick </td>
   <td style="text-align:left;"> couverture_internet </td>
   <td style="text-align:right;"> 20 </td>
   <td style="text-align:right;"> 351 </td>
   <td style="text-align:right;"> 5.698006 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Fatick </td>
   <td style="text-align:left;"> couverture_telephone </td>
   <td style="text-align:right;"> 101 </td>
   <td style="text-align:right;"> 351 </td>
   <td style="text-align:right;"> 28.774929 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Fatick </td>
   <td style="text-align:left;"> electricite_public </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 351 </td>
   <td style="text-align:right;"> 0.000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Fatick </td>
   <td style="text-align:left;"> magasinage_prod_site </td>
   <td style="text-align:right;"> 15 </td>
   <td style="text-align:right;"> 351 </td>
   <td style="text-align:right;"> 4.273504 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Fatick </td>
   <td style="text-align:left;"> robinet </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 351 </td>
   <td style="text-align:right;"> 0.000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Fatick </td>
   <td style="text-align:left;"> site_amenage_am </td>
   <td style="text-align:right;"> 66 </td>
   <td style="text-align:right;"> 351 </td>
   <td style="text-align:right;"> 18.803419 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Kaolack </td>
   <td style="text-align:left;"> magasinage_prod_site </td>
   <td style="text-align:right;"> 10 </td>
   <td style="text-align:right;"> 10 </td>
   <td style="text-align:right;"> 100.000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Louga </td>
   <td style="text-align:left;"> electricite_public </td>
   <td style="text-align:right;"> 5 </td>
   <td style="text-align:right;"> 200 </td>
   <td style="text-align:right;"> 2.500000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Louga </td>
   <td style="text-align:left;"> latrine_douche </td>
   <td style="text-align:right;"> 5 </td>
   <td style="text-align:right;"> 200 </td>
   <td style="text-align:right;"> 2.500000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Louga </td>
   <td style="text-align:left;"> magasinage_prod_site </td>
   <td style="text-align:right;"> 5 </td>
   <td style="text-align:right;"> 200 </td>
   <td style="text-align:right;"> 2.500000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Louga </td>
   <td style="text-align:left;"> robinet </td>
   <td style="text-align:right;"> 160 </td>
   <td style="text-align:right;"> 200 </td>
   <td style="text-align:right;"> 80.000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Saint Louis </td>
   <td style="text-align:left;"> approv_combust </td>
   <td style="text-align:right;"> 300 </td>
   <td style="text-align:right;"> 361 </td>
   <td style="text-align:right;"> 83.102493 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Saint Louis </td>
   <td style="text-align:left;"> couverture_internet </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 361 </td>
   <td style="text-align:right;"> 0.000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Saint Louis </td>
   <td style="text-align:left;"> couverture_telephone </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 361 </td>
   <td style="text-align:right;"> 0.000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Saint Louis </td>
   <td style="text-align:left;"> electricite_public </td>
   <td style="text-align:right;"> 100 </td>
   <td style="text-align:right;"> 361 </td>
   <td style="text-align:right;"> 27.700831 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Thies </td>
   <td style="text-align:left;"> boutiq_mat_transf </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 1434 </td>
   <td style="text-align:right;"> 0.000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Thies </td>
   <td style="text-align:left;"> certificat_site </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 1434 </td>
   <td style="text-align:right;"> 0.000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Thies </td>
   <td style="text-align:left;"> couverture_internet </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 1434 </td>
   <td style="text-align:right;"> 0.000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Thies </td>
   <td style="text-align:left;"> couverture_telephone </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 1434 </td>
   <td style="text-align:right;"> 0.000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Thies </td>
   <td style="text-align:left;"> electricite_public </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 1434 </td>
   <td style="text-align:right;"> 0.000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Thies </td>
   <td style="text-align:left;"> latrine_douche </td>
   <td style="text-align:right;"> 15 </td>
   <td style="text-align:right;"> 1434 </td>
   <td style="text-align:right;"> 1.046025 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Thies </td>
   <td style="text-align:left;"> magasinage_prod_site </td>
   <td style="text-align:right;"> 115 </td>
   <td style="text-align:right;"> 1434 </td>
   <td style="text-align:right;"> 8.019526 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Thies </td>
   <td style="text-align:left;"> parking </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 1434 </td>
   <td style="text-align:right;"> 0.000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Thies </td>
   <td style="text-align:left;"> robinet </td>
   <td style="text-align:right;"> 15 </td>
   <td style="text-align:right;"> 1434 </td>
   <td style="text-align:right;"> 1.046025 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Thies </td>
   <td style="text-align:left;"> site_amenage_am </td>
   <td style="text-align:right;"> 15 </td>
   <td style="text-align:right;"> 1434 </td>
   <td style="text-align:right;"> 1.046025 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Ziguinchor </td>
   <td style="text-align:left;"> boutiq_mat_transf </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 2142 </td>
   <td style="text-align:right;"> 0.000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Ziguinchor </td>
   <td style="text-align:left;"> couverture_internet </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 2142 </td>
   <td style="text-align:right;"> 0.000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Ziguinchor </td>
   <td style="text-align:left;"> couverture_telephone </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 2142 </td>
   <td style="text-align:right;"> 0.000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Ziguinchor </td>
   <td style="text-align:left;"> electricite_public </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 2142 </td>
   <td style="text-align:right;"> 0.000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Ziguinchor </td>
   <td style="text-align:left;"> latrine_douche </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 2142 </td>
   <td style="text-align:right;"> 0.000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Ziguinchor </td>
   <td style="text-align:left;"> magasinage_prod_site </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 2142 </td>
   <td style="text-align:right;"> 0.000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Ziguinchor </td>
   <td style="text-align:left;"> parking </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 2142 </td>
   <td style="text-align:right;"> 0.000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Ziguinchor </td>
   <td style="text-align:left;"> robinet </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 2142 </td>
   <td style="text-align:right;"> 0.000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Ziguinchor </td>
   <td style="text-align:left;"> site_amenage_am </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 2142 </td>
   <td style="text-align:right;"> 0.000000 </td>
  </tr>
</tbody>
</table>



```r
ggplot(commodites.reg)+
  geom_bar(stat='identity',aes(x=Commodites,y=pour.Effectif_Operat))+
  facet_wrap(~niveau_administr_01)+
   theme(axis.text.x = element_text(angle = 90, vjust = 0.5, hjust=1))
```

<img src="/tmp/RtmpItTkgh/preview-2658e22e89d0f4.dir/Rapports_base2_files/figure-html/unnamed-chunk-56-1.png" width="672" />



### professionnels du site qui prennent poissons transformés du site pour aller vendre ailleurs ayant accès aux commodités (Présent et fonctionne) Pays


```r
#total<-dim(liste.sites.init)[1]
total<-sum(liste.sites.init[,49], na.rm = TRUE)

liste.sites.init[,c(1,14,seq(30,41),49)]  %>%
  pivot_longer( cols = names(liste.sites)[c(seq(30,41))], names_to = "Commodites") %>%
  filter(value=='Présent et fonctionne') %>%
#  filter(value=='Existe mais ne  fonctionne pas bien ou pas du tout') %>%
#  filter(value=='Présent et fonctionne' | value=='Existe mais ne  fonctionne pas bien ou pas du tout') %>%
  group_by(Commodites) %>%
  summarize(Effectif_Operat=sum(combien_person_pois_transf, na.rm = TRUE))%>%
#  donnees <- liste.sites.init(liste.sites.init[,c(seq(30,41))], effectif)
  
  
 mutate(pour.Effectif_Operat=Effectif_Operat/total*100) -> commodites

commodites %>%
  kbl() %>% kable_styling()
```

<table class="table" style="margin-left: auto; margin-right: auto;">
 <thead>
  <tr>
   <th style="text-align:left;"> Commodites </th>
   <th style="text-align:right;"> Effectif_Operat </th>
   <th style="text-align:right;"> pour.Effectif_Operat </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:left;"> approv_combust </td>
   <td style="text-align:right;"> 315 </td>
   <td style="text-align:right;"> 42.281879 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> boutiq_mat_transf </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 0.000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> certificat_site </td>
   <td style="text-align:right;"> 545 </td>
   <td style="text-align:right;"> 73.154362 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> couverture_internet </td>
   <td style="text-align:right;"> 444 </td>
   <td style="text-align:right;"> 59.597315 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> couverture_telephone </td>
   <td style="text-align:right;"> 449 </td>
   <td style="text-align:right;"> 60.268456 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> electricite_public </td>
   <td style="text-align:right;"> 399 </td>
   <td style="text-align:right;"> 53.557047 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> latrine_douche </td>
   <td style="text-align:right;"> 188 </td>
   <td style="text-align:right;"> 25.234899 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> magasinage_prod_site </td>
   <td style="text-align:right;"> 228 </td>
   <td style="text-align:right;"> 30.604027 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> parking </td>
   <td style="text-align:right;"> 23 </td>
   <td style="text-align:right;"> 3.087248 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> ramassage_traitem_dechets </td>
   <td style="text-align:right;"> 81 </td>
   <td style="text-align:right;"> 10.872483 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> robinet </td>
   <td style="text-align:right;"> 312 </td>
   <td style="text-align:right;"> 41.879195 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> site_amenage_am </td>
   <td style="text-align:right;"> 219 </td>
   <td style="text-align:right;"> 29.395973 </td>
  </tr>
</tbody>
</table>



```r
ggplot(commodites)+
  geom_bar(stat='identity',aes(x=Commodites,y=pour.Effectif_Operat))+
   theme(axis.text.x = element_text(angle = 90, vjust = 0.5, hjust=1))
```

<img src="/tmp/RtmpItTkgh/preview-2658e22e89d0f4.dir/Rapports_base2_files/figure-html/unnamed-chunk-58-1.png" width="672" />
A REVOIR
### professionnels du qui prennent poissons transformés du site pour aller vendre ailleurs ayant accès aux commodités (Présent et fonctionne) Région


```r
total.reg<- liste.sites.init %>% 
#total.reg <-sum(liste.sites.init[,47], na.rm = TRUE)
 group_by(niveau_administr_01) %>%
dplyr::summarize(total.reg=sum(combien_person_pois_transf, na.rm = TRUE))

liste.sites.init[,c(1,12,14,seq(30,41),49)]  %>%
  pivot_longer( cols = names(liste.sites)[c(seq(30,41))], names_to = "Commodites") %>%
  filter(value=='Présent et fonctionne') %>%
  group_by(niveau_administr_01,Commodites) %>%
  summarize(Effectif_Operat=sum(combien_person_pois_transf, na.rm = TRUE)) %>% 
  
  inner_join(total.reg) %>%
   dplyr::mutate(pour.Effectif_Operat=100*Effectif_Operat/total.reg)->commodites.reg
```

```
## `summarise()` has grouped output by 'niveau_administr_01'. You can override
## using the `.groups` argument.
## Joining, by = "niveau_administr_01"
```

```r
commodites.reg %>%
  kbl() %>% kable_styling()
```

<table class="table" style="margin-left: auto; margin-right: auto;">
 <thead>
  <tr>
   <th style="text-align:left;"> niveau_administr_01 </th>
   <th style="text-align:left;"> Commodites </th>
   <th style="text-align:right;"> Effectif_Operat </th>
   <th style="text-align:right;"> total.reg </th>
   <th style="text-align:right;"> pour.Effectif_Operat </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:left;"> Dakar </td>
   <td style="text-align:left;"> approv_combust </td>
   <td style="text-align:right;"> 180 </td>
   <td style="text-align:right;"> 180 </td>
   <td style="text-align:right;"> 100.000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Dakar </td>
   <td style="text-align:left;"> boutiq_mat_transf </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 180 </td>
   <td style="text-align:right;"> 0.000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Dakar </td>
   <td style="text-align:left;"> certificat_site </td>
   <td style="text-align:right;"> 180 </td>
   <td style="text-align:right;"> 180 </td>
   <td style="text-align:right;"> 100.000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Dakar </td>
   <td style="text-align:left;"> couverture_internet </td>
   <td style="text-align:right;"> 30 </td>
   <td style="text-align:right;"> 180 </td>
   <td style="text-align:right;"> 16.666667 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Dakar </td>
   <td style="text-align:left;"> couverture_telephone </td>
   <td style="text-align:right;"> 30 </td>
   <td style="text-align:right;"> 180 </td>
   <td style="text-align:right;"> 16.666667 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Dakar </td>
   <td style="text-align:left;"> electricite_public </td>
   <td style="text-align:right;"> 180 </td>
   <td style="text-align:right;"> 180 </td>
   <td style="text-align:right;"> 100.000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Dakar </td>
   <td style="text-align:left;"> latrine_douche </td>
   <td style="text-align:right;"> 6 </td>
   <td style="text-align:right;"> 180 </td>
   <td style="text-align:right;"> 3.333333 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Dakar </td>
   <td style="text-align:left;"> magasinage_prod_site </td>
   <td style="text-align:right;"> 180 </td>
   <td style="text-align:right;"> 180 </td>
   <td style="text-align:right;"> 100.000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Dakar </td>
   <td style="text-align:left;"> parking </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 180 </td>
   <td style="text-align:right;"> 0.000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Dakar </td>
   <td style="text-align:left;"> ramassage_traitem_dechets </td>
   <td style="text-align:right;"> 30 </td>
   <td style="text-align:right;"> 180 </td>
   <td style="text-align:right;"> 16.666667 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Dakar </td>
   <td style="text-align:left;"> robinet </td>
   <td style="text-align:right;"> 150 </td>
   <td style="text-align:right;"> 180 </td>
   <td style="text-align:right;"> 83.333333 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Dakar </td>
   <td style="text-align:left;"> site_amenage_am </td>
   <td style="text-align:right;"> 30 </td>
   <td style="text-align:right;"> 180 </td>
   <td style="text-align:right;"> 16.666667 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Fatick </td>
   <td style="text-align:left;"> approv_combust </td>
   <td style="text-align:right;"> 5 </td>
   <td style="text-align:right;"> 118 </td>
   <td style="text-align:right;"> 4.237288 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Fatick </td>
   <td style="text-align:left;"> boutiq_mat_transf </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 118 </td>
   <td style="text-align:right;"> 0.000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Fatick </td>
   <td style="text-align:left;"> certificat_site </td>
   <td style="text-align:right;"> 31 </td>
   <td style="text-align:right;"> 118 </td>
   <td style="text-align:right;"> 26.271186 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Fatick </td>
   <td style="text-align:left;"> couverture_internet </td>
   <td style="text-align:right;"> 47 </td>
   <td style="text-align:right;"> 118 </td>
   <td style="text-align:right;"> 39.830509 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Fatick </td>
   <td style="text-align:left;"> couverture_telephone </td>
   <td style="text-align:right;"> 52 </td>
   <td style="text-align:right;"> 118 </td>
   <td style="text-align:right;"> 44.067797 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Fatick </td>
   <td style="text-align:left;"> electricite_public </td>
   <td style="text-align:right;"> 29 </td>
   <td style="text-align:right;"> 118 </td>
   <td style="text-align:right;"> 24.576271 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Fatick </td>
   <td style="text-align:left;"> latrine_douche </td>
   <td style="text-align:right;"> 52 </td>
   <td style="text-align:right;"> 118 </td>
   <td style="text-align:right;"> 44.067797 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Fatick </td>
   <td style="text-align:left;"> magasinage_prod_site </td>
   <td style="text-align:right;"> 28 </td>
   <td style="text-align:right;"> 118 </td>
   <td style="text-align:right;"> 23.728814 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Fatick </td>
   <td style="text-align:left;"> parking </td>
   <td style="text-align:right;"> 23 </td>
   <td style="text-align:right;"> 118 </td>
   <td style="text-align:right;"> 19.491525 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Fatick </td>
   <td style="text-align:left;"> ramassage_traitem_dechets </td>
   <td style="text-align:right;"> 28 </td>
   <td style="text-align:right;"> 118 </td>
   <td style="text-align:right;"> 23.728814 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Fatick </td>
   <td style="text-align:left;"> robinet </td>
   <td style="text-align:right;"> 52 </td>
   <td style="text-align:right;"> 118 </td>
   <td style="text-align:right;"> 44.067797 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Fatick </td>
   <td style="text-align:left;"> site_amenage_am </td>
   <td style="text-align:right;"> 52 </td>
   <td style="text-align:right;"> 118 </td>
   <td style="text-align:right;"> 44.067797 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Kaolack </td>
   <td style="text-align:left;"> approv_combust </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> NaN </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Kaolack </td>
   <td style="text-align:left;"> boutiq_mat_transf </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> NaN </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Kaolack </td>
   <td style="text-align:left;"> couverture_internet </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> NaN </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Kaolack </td>
   <td style="text-align:left;"> couverture_telephone </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> NaN </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Kaolack </td>
   <td style="text-align:left;"> electricite_public </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> NaN </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Kaolack </td>
   <td style="text-align:left;"> latrine_douche </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> NaN </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Kaolack </td>
   <td style="text-align:left;"> magasinage_prod_site </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> NaN </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Kaolack </td>
   <td style="text-align:left;"> parking </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> NaN </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Kaolack </td>
   <td style="text-align:left;"> site_amenage_am </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> NaN </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Louga </td>
   <td style="text-align:left;"> approv_combust </td>
   <td style="text-align:right;"> 20 </td>
   <td style="text-align:right;"> 44 </td>
   <td style="text-align:right;"> 45.454546 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Louga </td>
   <td style="text-align:left;"> certificat_site </td>
   <td style="text-align:right;"> 44 </td>
   <td style="text-align:right;"> 44 </td>
   <td style="text-align:right;"> 100.000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Louga </td>
   <td style="text-align:left;"> couverture_internet </td>
   <td style="text-align:right;"> 44 </td>
   <td style="text-align:right;"> 44 </td>
   <td style="text-align:right;"> 100.000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Louga </td>
   <td style="text-align:left;"> couverture_telephone </td>
   <td style="text-align:right;"> 44 </td>
   <td style="text-align:right;"> 44 </td>
   <td style="text-align:right;"> 100.000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Louga </td>
   <td style="text-align:left;"> electricite_public </td>
   <td style="text-align:right;"> 20 </td>
   <td style="text-align:right;"> 44 </td>
   <td style="text-align:right;"> 45.454546 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Louga </td>
   <td style="text-align:left;"> latrine_douche </td>
   <td style="text-align:right;"> 20 </td>
   <td style="text-align:right;"> 44 </td>
   <td style="text-align:right;"> 45.454546 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Louga </td>
   <td style="text-align:left;"> magasinage_prod_site </td>
   <td style="text-align:right;"> 20 </td>
   <td style="text-align:right;"> 44 </td>
   <td style="text-align:right;"> 45.454546 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Louga </td>
   <td style="text-align:left;"> ramassage_traitem_dechets </td>
   <td style="text-align:right;"> 20 </td>
   <td style="text-align:right;"> 44 </td>
   <td style="text-align:right;"> 45.454546 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Louga </td>
   <td style="text-align:left;"> site_amenage_am </td>
   <td style="text-align:right;"> 27 </td>
   <td style="text-align:right;"> 44 </td>
   <td style="text-align:right;"> 61.363636 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Saint Louis </td>
   <td style="text-align:left;"> approv_combust </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 210 </td>
   <td style="text-align:right;"> 0.000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Saint Louis </td>
   <td style="text-align:left;"> certificat_site </td>
   <td style="text-align:right;"> 210 </td>
   <td style="text-align:right;"> 210 </td>
   <td style="text-align:right;"> 100.000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Saint Louis </td>
   <td style="text-align:left;"> couverture_internet </td>
   <td style="text-align:right;"> 210 </td>
   <td style="text-align:right;"> 210 </td>
   <td style="text-align:right;"> 100.000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Saint Louis </td>
   <td style="text-align:left;"> couverture_telephone </td>
   <td style="text-align:right;"> 210 </td>
   <td style="text-align:right;"> 210 </td>
   <td style="text-align:right;"> 100.000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Saint Louis </td>
   <td style="text-align:left;"> electricite_public </td>
   <td style="text-align:right;"> 60 </td>
   <td style="text-align:right;"> 210 </td>
   <td style="text-align:right;"> 28.571429 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Saint Louis </td>
   <td style="text-align:left;"> latrine_douche </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 210 </td>
   <td style="text-align:right;"> 0.000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Saint Louis </td>
   <td style="text-align:left;"> magasinage_prod_site </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 210 </td>
   <td style="text-align:right;"> 0.000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Saint Louis </td>
   <td style="text-align:left;"> parking </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 210 </td>
   <td style="text-align:right;"> 0.000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Saint Louis </td>
   <td style="text-align:left;"> site_amenage_am </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 210 </td>
   <td style="text-align:right;"> 0.000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Thies </td>
   <td style="text-align:left;"> approv_combust </td>
   <td style="text-align:right;"> 110 </td>
   <td style="text-align:right;"> 113 </td>
   <td style="text-align:right;"> 97.345133 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Thies </td>
   <td style="text-align:left;"> certificat_site </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 113 </td>
   <td style="text-align:right;"> 0.000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Thies </td>
   <td style="text-align:left;"> couverture_internet </td>
   <td style="text-align:right;"> 113 </td>
   <td style="text-align:right;"> 113 </td>
   <td style="text-align:right;"> 100.000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Thies </td>
   <td style="text-align:left;"> couverture_telephone </td>
   <td style="text-align:right;"> 113 </td>
   <td style="text-align:right;"> 113 </td>
   <td style="text-align:right;"> 100.000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Thies </td>
   <td style="text-align:left;"> electricite_public </td>
   <td style="text-align:right;"> 110 </td>
   <td style="text-align:right;"> 113 </td>
   <td style="text-align:right;"> 97.345133 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Thies </td>
   <td style="text-align:left;"> latrine_douche </td>
   <td style="text-align:right;"> 110 </td>
   <td style="text-align:right;"> 113 </td>
   <td style="text-align:right;"> 97.345133 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Thies </td>
   <td style="text-align:left;"> magasinage_prod_site </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 113 </td>
   <td style="text-align:right;"> 0.000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Thies </td>
   <td style="text-align:left;"> parking </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 113 </td>
   <td style="text-align:right;"> 0.000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Thies </td>
   <td style="text-align:left;"> ramassage_traitem_dechets </td>
   <td style="text-align:right;"> 3 </td>
   <td style="text-align:right;"> 113 </td>
   <td style="text-align:right;"> 2.654867 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Thies </td>
   <td style="text-align:left;"> robinet </td>
   <td style="text-align:right;"> 110 </td>
   <td style="text-align:right;"> 113 </td>
   <td style="text-align:right;"> 97.345133 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Thies </td>
   <td style="text-align:left;"> site_amenage_am </td>
   <td style="text-align:right;"> 110 </td>
   <td style="text-align:right;"> 113 </td>
   <td style="text-align:right;"> 97.345133 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Ziguinchor </td>
   <td style="text-align:left;"> approv_combust </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 80 </td>
   <td style="text-align:right;"> 0.000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Ziguinchor </td>
   <td style="text-align:left;"> boutiq_mat_transf </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 80 </td>
   <td style="text-align:right;"> 0.000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Ziguinchor </td>
   <td style="text-align:left;"> certificat_site </td>
   <td style="text-align:right;"> 80 </td>
   <td style="text-align:right;"> 80 </td>
   <td style="text-align:right;"> 100.000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Ziguinchor </td>
   <td style="text-align:left;"> couverture_internet </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 80 </td>
   <td style="text-align:right;"> 0.000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Ziguinchor </td>
   <td style="text-align:left;"> couverture_telephone </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 80 </td>
   <td style="text-align:right;"> 0.000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Ziguinchor </td>
   <td style="text-align:left;"> electricite_public </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 80 </td>
   <td style="text-align:right;"> 0.000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Ziguinchor </td>
   <td style="text-align:left;"> latrine_douche </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 80 </td>
   <td style="text-align:right;"> 0.000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Ziguinchor </td>
   <td style="text-align:left;"> magasinage_prod_site </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 80 </td>
   <td style="text-align:right;"> 0.000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Ziguinchor </td>
   <td style="text-align:left;"> ramassage_traitem_dechets </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 80 </td>
   <td style="text-align:right;"> 0.000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Ziguinchor </td>
   <td style="text-align:left;"> robinet </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 80 </td>
   <td style="text-align:right;"> 0.000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Ziguinchor </td>
   <td style="text-align:left;"> site_amenage_am </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 80 </td>
   <td style="text-align:right;"> 0.000000 </td>
  </tr>
</tbody>
</table>



```r
ggplot(commodites.reg)+
  geom_bar(stat='identity',aes(x=Commodites,y=pour.Effectif_Operat))+
  facet_wrap(~niveau_administr_01)+
   theme(axis.text.x = element_text(angle = 90, vjust = 0.5, hjust=1))
```

```
## Warning: Removed 9 rows containing missing values (`position_stack()`).
```

<img src="/tmp/RtmpItTkgh/preview-2658e22e89d0f4.dir/Rapports_base2_files/figure-html/unnamed-chunk-60-1.png" width="672" />



### professionnels du site qui prennent poissons transformés du site pour aller vendre ailleurs ayant accès aux commodités (Existe mais ne  fonctionne pas bien ou pas du tout) Pays


```r
#total<-dim(liste.sites.init)[1]
total<-sum(liste.sites.init[,49], na.rm = TRUE)

liste.sites.init[,c(1,14,seq(30,41),49)]  %>%
  pivot_longer( cols = names(liste.sites)[c(seq(30,41))], names_to = "Commodites") %>%
 # filter(value=='Présent et fonctionne') %>%
  filter(value=='Existe mais ne  fonctionne pas bien ou pas du tout') %>%
#  filter(value=='Présent et fonctionne' | value=='Existe mais ne  fonctionne pas bien ou pas du tout') %>%
  group_by(Commodites) %>%
  summarize(Effectif_Operat=sum(combien_person_pois_transf, na.rm = TRUE))%>%
#  donnees <- liste.sites.init(liste.sites.init[,c(seq(30,41))], effectif)
  
  
 mutate(pour.Effectif_Operat=Effectif_Operat/total*100) -> commodites

commodites %>%
  kbl() %>% kable_styling()
```

<table class="table" style="margin-left: auto; margin-right: auto;">
 <thead>
  <tr>
   <th style="text-align:left;"> Commodites </th>
   <th style="text-align:right;"> Effectif_Operat </th>
   <th style="text-align:right;"> pour.Effectif_Operat </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:left;"> approv_combust </td>
   <td style="text-align:right;"> 150 </td>
   <td style="text-align:right;"> 20.1342282 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> boutiq_mat_transf </td>
   <td style="text-align:right;"> 80 </td>
   <td style="text-align:right;"> 10.7382550 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> certificat_site </td>
   <td style="text-align:right;"> 5 </td>
   <td style="text-align:right;"> 0.6711409 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> couverture_internet </td>
   <td style="text-align:right;"> 88 </td>
   <td style="text-align:right;"> 11.8120805 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> couverture_telephone </td>
   <td style="text-align:right;"> 146 </td>
   <td style="text-align:right;"> 19.5973154 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> electricite_public </td>
   <td style="text-align:right;"> 137 </td>
   <td style="text-align:right;"> 18.3892617 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> latrine_douche </td>
   <td style="text-align:right;"> 261 </td>
   <td style="text-align:right;"> 35.0335570 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> magasinage_prod_site </td>
   <td style="text-align:right;"> 197 </td>
   <td style="text-align:right;"> 26.4429530 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> parking </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 0.0000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> robinet </td>
   <td style="text-align:right;"> 100 </td>
   <td style="text-align:right;"> 13.4228188 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> site_amenage_am </td>
   <td style="text-align:right;"> 208 </td>
   <td style="text-align:right;"> 27.9194631 </td>
  </tr>
</tbody>
</table>



```r
ggplot(commodites)+
  geom_bar(stat='identity',aes(x=Commodites,y=pour.Effectif_Operat))+
   theme(axis.text.x = element_text(angle = 90, vjust = 0.5, hjust=1))
```

<img src="/tmp/RtmpItTkgh/preview-2658e22e89d0f4.dir/Rapports_base2_files/figure-html/unnamed-chunk-62-1.png" width="672" />
A REVOIR
### professionnels du qui prennent poissons transformés du site pour aller vendre ailleurs ayant accès aux commodités (Existe mais ne  fonctionne pas bien ou pas du tout) Région


```r
total.reg<- liste.sites.init %>% 
#total.reg <-sum(liste.sites.init[,47], na.rm = TRUE)
 group_by(niveau_administr_01) %>%
dplyr::summarize(total.reg=sum(combien_person_pois_transf, na.rm = TRUE))

liste.sites.init[,c(1,12,14,seq(30,41),49)]  %>%
  pivot_longer( cols = names(liste.sites)[c(seq(30,41))], names_to = "Commodites") %>%
  filter(value=='Existe mais ne  fonctionne pas bien ou pas du tout') %>%
  group_by(niveau_administr_01,Commodites) %>%
  summarize(Effectif_Operat=sum(combien_person_pois_transf, na.rm = TRUE)) %>% 
  
  inner_join(total.reg) %>%
   dplyr::mutate(pour.Effectif_Operat=100*Effectif_Operat/total.reg)->commodites.reg
```

```
## `summarise()` has grouped output by 'niveau_administr_01'. You can override
## using the `.groups` argument.
## Joining, by = "niveau_administr_01"
```

```r
commodites.reg %>%
  kbl() %>% kable_styling()
```

<table class="table" style="margin-left: auto; margin-right: auto;">
 <thead>
  <tr>
   <th style="text-align:left;"> niveau_administr_01 </th>
   <th style="text-align:left;"> Commodites </th>
   <th style="text-align:right;"> Effectif_Operat </th>
   <th style="text-align:right;"> total.reg </th>
   <th style="text-align:right;"> pour.Effectif_Operat </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:left;"> Dakar </td>
   <td style="text-align:left;"> latrine_douche </td>
   <td style="text-align:right;"> 174 </td>
   <td style="text-align:right;"> 180 </td>
   <td style="text-align:right;"> 96.666667 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Dakar </td>
   <td style="text-align:left;"> site_amenage_am </td>
   <td style="text-align:right;"> 150 </td>
   <td style="text-align:right;"> 180 </td>
   <td style="text-align:right;"> 83.333333 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Fatick </td>
   <td style="text-align:left;"> boutiq_mat_transf </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 118 </td>
   <td style="text-align:right;"> 0.000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Fatick </td>
   <td style="text-align:left;"> certificat_site </td>
   <td style="text-align:right;"> 5 </td>
   <td style="text-align:right;"> 118 </td>
   <td style="text-align:right;"> 4.237288 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Fatick </td>
   <td style="text-align:left;"> couverture_internet </td>
   <td style="text-align:right;"> 8 </td>
   <td style="text-align:right;"> 118 </td>
   <td style="text-align:right;"> 6.779661 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Fatick </td>
   <td style="text-align:left;"> couverture_telephone </td>
   <td style="text-align:right;"> 66 </td>
   <td style="text-align:right;"> 118 </td>
   <td style="text-align:right;"> 55.932203 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Fatick </td>
   <td style="text-align:left;"> electricite_public </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 118 </td>
   <td style="text-align:right;"> 0.000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Fatick </td>
   <td style="text-align:left;"> magasinage_prod_site </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 118 </td>
   <td style="text-align:right;"> 0.000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Fatick </td>
   <td style="text-align:left;"> robinet </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 118 </td>
   <td style="text-align:right;"> 0.000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Fatick </td>
   <td style="text-align:left;"> site_amenage_am </td>
   <td style="text-align:right;"> 58 </td>
   <td style="text-align:right;"> 118 </td>
   <td style="text-align:right;"> 49.152542 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Kaolack </td>
   <td style="text-align:left;"> magasinage_prod_site </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> NaN </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Louga </td>
   <td style="text-align:left;"> electricite_public </td>
   <td style="text-align:right;"> 7 </td>
   <td style="text-align:right;"> 44 </td>
   <td style="text-align:right;"> 15.909091 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Louga </td>
   <td style="text-align:left;"> latrine_douche </td>
   <td style="text-align:right;"> 7 </td>
   <td style="text-align:right;"> 44 </td>
   <td style="text-align:right;"> 15.909091 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Louga </td>
   <td style="text-align:left;"> magasinage_prod_site </td>
   <td style="text-align:right;"> 7 </td>
   <td style="text-align:right;"> 44 </td>
   <td style="text-align:right;"> 15.909091 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Louga </td>
   <td style="text-align:left;"> robinet </td>
   <td style="text-align:right;"> 20 </td>
   <td style="text-align:right;"> 44 </td>
   <td style="text-align:right;"> 45.454546 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Saint Louis </td>
   <td style="text-align:left;"> approv_combust </td>
   <td style="text-align:right;"> 150 </td>
   <td style="text-align:right;"> 210 </td>
   <td style="text-align:right;"> 71.428571 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Saint Louis </td>
   <td style="text-align:left;"> couverture_internet </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 210 </td>
   <td style="text-align:right;"> 0.000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Saint Louis </td>
   <td style="text-align:left;"> couverture_telephone </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 210 </td>
   <td style="text-align:right;"> 0.000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Saint Louis </td>
   <td style="text-align:left;"> electricite_public </td>
   <td style="text-align:right;"> 50 </td>
   <td style="text-align:right;"> 210 </td>
   <td style="text-align:right;"> 23.809524 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Thies </td>
   <td style="text-align:left;"> boutiq_mat_transf </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 113 </td>
   <td style="text-align:right;"> 0.000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Thies </td>
   <td style="text-align:left;"> certificat_site </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 113 </td>
   <td style="text-align:right;"> 0.000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Thies </td>
   <td style="text-align:left;"> couverture_internet </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 113 </td>
   <td style="text-align:right;"> 0.000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Thies </td>
   <td style="text-align:left;"> couverture_telephone </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 113 </td>
   <td style="text-align:right;"> 0.000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Thies </td>
   <td style="text-align:left;"> electricite_public </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 113 </td>
   <td style="text-align:right;"> 0.000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Thies </td>
   <td style="text-align:left;"> latrine_douche </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 113 </td>
   <td style="text-align:right;"> 0.000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Thies </td>
   <td style="text-align:left;"> magasinage_prod_site </td>
   <td style="text-align:right;"> 110 </td>
   <td style="text-align:right;"> 113 </td>
   <td style="text-align:right;"> 97.345133 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Thies </td>
   <td style="text-align:left;"> parking </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 113 </td>
   <td style="text-align:right;"> 0.000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Thies </td>
   <td style="text-align:left;"> robinet </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 113 </td>
   <td style="text-align:right;"> 0.000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Thies </td>
   <td style="text-align:left;"> site_amenage_am </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 113 </td>
   <td style="text-align:right;"> 0.000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Ziguinchor </td>
   <td style="text-align:left;"> boutiq_mat_transf </td>
   <td style="text-align:right;"> 80 </td>
   <td style="text-align:right;"> 80 </td>
   <td style="text-align:right;"> 100.000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Ziguinchor </td>
   <td style="text-align:left;"> couverture_internet </td>
   <td style="text-align:right;"> 80 </td>
   <td style="text-align:right;"> 80 </td>
   <td style="text-align:right;"> 100.000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Ziguinchor </td>
   <td style="text-align:left;"> couverture_telephone </td>
   <td style="text-align:right;"> 80 </td>
   <td style="text-align:right;"> 80 </td>
   <td style="text-align:right;"> 100.000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Ziguinchor </td>
   <td style="text-align:left;"> electricite_public </td>
   <td style="text-align:right;"> 80 </td>
   <td style="text-align:right;"> 80 </td>
   <td style="text-align:right;"> 100.000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Ziguinchor </td>
   <td style="text-align:left;"> latrine_douche </td>
   <td style="text-align:right;"> 80 </td>
   <td style="text-align:right;"> 80 </td>
   <td style="text-align:right;"> 100.000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Ziguinchor </td>
   <td style="text-align:left;"> magasinage_prod_site </td>
   <td style="text-align:right;"> 80 </td>
   <td style="text-align:right;"> 80 </td>
   <td style="text-align:right;"> 100.000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Ziguinchor </td>
   <td style="text-align:left;"> parking </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 80 </td>
   <td style="text-align:right;"> 0.000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Ziguinchor </td>
   <td style="text-align:left;"> robinet </td>
   <td style="text-align:right;"> 80 </td>
   <td style="text-align:right;"> 80 </td>
   <td style="text-align:right;"> 100.000000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Ziguinchor </td>
   <td style="text-align:left;"> site_amenage_am </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 80 </td>
   <td style="text-align:right;"> 0.000000 </td>
  </tr>
</tbody>
</table>



```r
ggplot(commodites.reg)+
  geom_bar(stat='identity',aes(x=Commodites,y=pour.Effectif_Operat))+
  facet_wrap(~niveau_administr_01)+
   theme(axis.text.x = element_text(angle = 90, vjust = 0.5, hjust=1))
```

```
## Warning: Removed 1 rows containing missing values (`position_stack()`).
```

<img src="/tmp/RtmpItTkgh/preview-2658e22e89d0f4.dir/Rapports_base2_files/figure-html/unnamed-chunk-64-1.png" width="672" />




### Nombre de focus group

C1-Y-a-t-il en cette période (mois) des commerçant(e)s qui achètent ici du poisson frais aux pêcheurs et le revendent au détail sur le site même ou dans la localité voisine (= micro-mareyeuse) ?     


C-2 Y-a-t-il en cette période (mois) des commerçant(e)s qui achètent ici du poisson frais aux pêcheurs et le revendent en gros ou demi-gros à des transporteurs qui quittent le site ou bien vont-elles-mêmes le revendre ailleurs dans des localités distantes (= mareyeurs ou mareyeuses) ?


C3- Y-a-t-il en cette période (mois) des personnes, mandatées par des usines, qui achètent ici du poisson frais aux pêcheurs pour alimenter en produits des usines proches ou lointaines (= 
acheteurs (ses)) ?     

<!-- ```{r} -->


<!-- totaux<-donnees$comm_sites %>% select(niveau_administr_01,nbr_micro_marey_c1_revente_site,nbr_marey_c2_revte_site,nbr_acheteur_c3_revte_site) -->

<!-- nb_fiche.c1<-donnees$comm_sites %>% inner_join(donnees$comm_poisson_frais_c1,by=c('num_fiche_enq','date_enq')) %>%  -->
<!--   dplyr::group_by(niveau_administr_01) %>%  -->
<!--   dplyr::summarise(nb_enquete.c1=n()) -->

<!-- nb_fiche.c2<-donnees$comm_sites %>% inner_join(donnees$comm_poisson_frais_c2,by=c('num_fiche_enq','date_enq')) %>%  -->
<!--   dplyr::group_by(niveau_administr_01) %>%  -->
<!--   dplyr::summarise(nb_enquete.c2=n()) -->

<!-- nb_fiche.c3<-donnees$comm_sites %>% inner_join(donnees$comm_poisson_frais_c3,by=c('num_fiche_enq','date_enq')) %>%  -->
<!--   dplyr::group_by(niveau_administr_01) %>%  -->
<!--   dplyr::summarise(nb_enquete.c3=n()) -->

<!-- totaux %>% left_join(nb_fiche.c1) %>%  -->
<!--   left_join(nb_fiche.c2) %>%  -->
<!--   left_join(nb_fiche.c3) %>%  -->
<!-- pivot_longer(!niveau_administr_01,names_to = "donnee") %>%  -->
<!--   ggplot()+geom_bar(stat='identity',aes(x=donnee,fill=niveau_administr_01,y=value),position=position_dodge()) %>%  -->


<!-- totaux %>% left_join(nb_fiche.c1) %>%  -->
<!--   left_join(nb_fiche.c2) %>%  -->
<!--   left_join(nb_fiche.c3) %>%  -->
<!-- pivot_longer(!niveau_administr_01,names_to = "donnee") %>%  -->
<!--   ggplot()+geom_bar(stat='identity',aes(x=niveau_administr_01,fill=donnee,y=value),position=position_dodge())  -->


<!-- ``` -->
