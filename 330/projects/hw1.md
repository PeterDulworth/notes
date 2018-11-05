# C330 HW 1 - PETER DULWORTH

```
DRINKS (PERSON, COFFEE) 

HAS_BEAN (COFFEE, BEAN_NAME) 

BEAN (BEAN_NAME, FROM_LOCATION) 
```

1. 

   - (a) Which beans are in the coffee named “Blend 101”? 

     $\pi_{BEAN\_NAME}(\sigma_{COFFEE='BLEND\ 101'}(HAS\_BEAN))$

   - (b) Which people drink a coffee that contains the “Pacas” coffee bean? 

     ```
     pi PERSON (sigma HAS_BEAN.BEAN_NAME='pacas' (DRINKS * HAS_BEAN))
     ```

     $\pi_{PERSON}(\sigma_{HAS\_BEAN.BEAN\_NAME='pacas'}(DRINKS * HAS\_BEAN))$  

   - (c) Who does not drink a coffee containing a bean from the location “Rwanda”? 

     ```
     x = DRINKS * HAS_BEAN * (sigma FROM_LOCATION ='rwanda' (BEAN))
     (pi PERSON (DRINKS)) - (pi PERSON x)
     ```

     $x\leftarrow DRINKS * HAS\_BEAN * (\sigma_{FROM\_LOCATION='rwanda'}(BEAN))$    

     $(\pi_{PERSON} (DRINKS)) - (\pi_{PERSON}(x))$  

2. Consider the same set of relations: 

   - (a) Which locations contribute a bean to the coffee named “Garuda blend”? 
     $$
     \{ b.FL \mid BEAN(b) \and \exists(hb)(HAS\_BEAN(hb) \and hb.C=Garuda Blend \and hb.BN=b.BN) \}
     $$

   - (b) Who has never tried a coffee containing a bean from the location “Hawaii”? 

     ```
     {d.P | DRINKS(d) and 
     FORALL(d2)(DRINKS(d2) and 
     	d.P=d2.P -> not EXISTS(hb)(HAS_BEAN(hb) and 				
     		hb.C=d.C and EXISTS(b)(BEAN(b) and 
     			hb.BN=b.BN and b.FL='Hawaii') ) )}
     ```

   - (c) Which people drink all of the coffees containing the bean “Caturra”?

     ```
     {d.PERSON | DRINKS(d) and FORALL(hb)(HAS_BEAN(hb) and hb.BEAN_NAME='Caturra' -> EXISTS(d2)(DRINKS(d2) and d2.PERSON=d.PERSON and d2.COFFEE=hb.COFFEE) ) )}
     ```
