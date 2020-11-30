Data Tidying & Analysis
================

Pull in data from Guttmacher and tidy (data from 2014-2017)

``` r
data = 
  read_csv("./data/112420_guttmacher data.csv")%>%
  select(state_id, measure_name, datum)%>%
  mutate(datum = as.double(datum))%>%
  pivot_wider(names_from = measure_name, values_from = datum)%>%
  janitor:: clean_names()%>%
  rename(public_expenditures = total_reported_public_expenditures_for_family_planning_client_services_in_000s_of_dollars,
         no_women_need_public_expenditure = no_of_women_who_likely_need_public_support_for_contraceptive_services_and_supplies_aged_13_44)%>%
  mutate(
    public_expenditures=as.double(public_expenditures),
    no_women_need_public_expenditure=as.double(no_women_need_public_expenditure),
    expenditure_rate = public_expenditures/no_women_need_public_expenditure
  )                                                                                #create expenditure_rate variable
```

    ## Parsed with column specification:
    ## cols(
    ##   measure_name = col_character(),
    ##   datum = col_character(),
    ##   state_id = col_character(),
    ##   state_name = col_character(),
    ##   first_year = col_double(),
    ##   last_year = col_character(),
    ##   footnotes = col_character(),
    ##   sources = col_character()
    ## )

    ## Warning: Problem with `mutate()` input `datum`.
    ## ℹ NAs introduced by coercion
    ## ℹ Input `datum` is `as.double(datum)`.

    ## Warning in mask$eval_all_mutate(dots[[i]]): NAs introduced by coercion

    ## Warning: Values are not uniquely identified; output will contain list-cols.
    ## * Use `values_fn = list` to suppress this warning.
    ## * Use `values_fn = length` to identify where the duplicates arise
    ## * Use `values_fn = {summary_fun}` to summarise duplicates

Load state hostility date (from 2010 data\*\* \[ \] complete)

``` r
hostility =
  read_csv("./data/state_hostility.csv")%>%
  separate('state_id,Hostility',c('state_id', 'hostility'), sep=",")%>%
  mutate(
    hostility = fct_relevel(hostility, c("hostile", "leans_hostile", "middle_ground", "leans_supportive", "supportive"))
  )
```

    ## Parsed with column specification:
    ## cols(
    ##   `state_id,Hostility` = col_character()
    ## )

### Merge data with hostility rank

### Rename variable names

\[1\] `state_id` = “state\_id”  
\[2\] `counties_no_provider` =
“percent\_of\_counties\_without\_a\_known\_abortion\_provider”  
\[3\] `percent_abortion` =
“percent\_of\_pregnancies\_ending\_in\_abortion”  
\[4\] `percent_birth` = “percent\_of\_pregnancies\_ending\_in\_birth”  
\[5\] `percent_medicaid` =
“percent\_of\_women\_aged\_15\_44\_covered\_by\_medicaid”  
\[6\] `percent_women_no_provider` =
“percent\_of\_women\_aged\_15\_44\_living\_in\_a\_county\_without\_an\_abortion\_provider”  
\[7\] `percent_uninsured` =
“percent\_of\_women\_aged\_15\_44\_who\_are\_uninsured”  
\[8\] `percent_bc_18_49` =
“percent\_of\_women\_aged\_18\_49\_using\_contraceptives”  
\[9\] `abortion_rate_15_17_state` =
“abortion\_rate\_the\_no\_of\_abortions\_per\_1\_000\_women\_aged\_15\_17\_by\_state\_of\_residence”  
\[10\] `abortion_rate_15_19_state` =
“abortion\_rate\_the\_no\_of\_abortions\_per\_1\_000\_women\_aged\_15\_19\_by\_state\_of\_residence”  
\[11\] `abortion_rate_15_44_state` =
“abortion\_rate\_the\_no\_of\_abortions\_per\_1\_000\_women\_aged\_15\_44\_by\_state\_of\_residence”  
\[12\] `abortion_rate_18_19_state` =
“abortion\_rate\_the\_no\_of\_abortions\_per\_1\_000\_women\_aged\_18\_19\_by\_state\_of\_residence”  
\[13\] `birthrate_15_17_state` =
“birthrate\_the\_no\_of\_births\_per\_1\_000\_women\_aged\_15\_17\_by\_state\_of\_residence”  
\[14\] `birthrate_15_19_state` =
“birthrate\_the\_no\_of\_births\_per\_1\_000\_women\_aged\_15\_19\_by\_state\_of\_residence”  
\[15\] `birthrate_18_19_state` =
“birthrate\_the\_no\_of\_births\_per\_1\_000\_women\_aged\_18\_19\_by\_state\_of\_residence”  
\[16\] `need_bc_hisp_20_44` =
“no\_of\_women\_who\_likely\_need\_public\_support\_for\_contraceptive\_services\_and\_supplies\_hispanic\_aged\_20\_44\_and\_below\_250\_percent\_of\_the\_federal\_poverty\_level”  
\[17\] `need_bc_hisp_younger_20` =
“no\_of\_women\_who\_likely\_need\_public\_support\_for\_contraceptive\_services\_and\_supplies\_hispanic\_younger\_than\_20”  
\[18\] `need_bc_13_44` =
“no\_of\_women\_who\_likely\_need\_public\_support\_for\_contraceptive\_services\_and\_supplies\_aged\_13\_44”  
\[19\] `need_bc_black_20_44` =
“no\_of\_women\_who\_likely\_need\_public\_support\_for\_contraceptive\_services\_and\_supplies\_non\_hispanic\_black\_aged\_20\_44\_and\_below\_250\_percent\_of\_the\_federal\_poverty\_level”
\[20\] `need_bc_black_under_20` =
“no\_of\_women\_who\_likely\_need\_public\_support\_for\_contraceptive\_services\_and\_supplies\_non\_hispanic\_black\_younger\_than\_20”  
\[21\] `need_bc_white_20_44` =
“no\_of\_women\_who\_likely\_need\_public\_support\_for\_contraceptive\_services\_and\_supplies\_non\_hispanic\_white\_aged\_20\_44\_and\_below\_250\_percent\_of\_the\_federal\_poverty\_level”
\[22\] `need_bc_white_under_20` =
“no\_of\_women\_who\_likely\_need\_public\_support\_for\_contraceptive\_services\_and\_supplies\_non\_hispanic\_white\_younger\_than\_20”  
\[23\] `need_bc_under_20` =
“no\_of\_women\_who\_likely\_need\_public\_support\_for\_contraceptive\_services\_and\_supplies\_younger\_than\_20”  
\[24\] `total_expend_abortion` =
“total\_reported\_public\_expenditures\_for\_abortions\_in\_000s\_of\_dollars”  
\[25\] `public_expenditures` =
“total\_reported\_public\_expenditures\_for\_family\_planning\_client\_services\_in\_000s\_of\_dollars”  
\[26\] `expenditure_rate` = “public\_expenditures”/“need\_bc\_13\_44”  
\[27\] `hostility` = state hostility ranking in 2000 according to
Guttmacher

``` r
merge_data = 
  merge(data, hostility, by = "state_id")%>%
  select(-percent_of_pregnancies_wanted_later_or_unwanted_ending_in_abortion, -percent_of_pregnancies_wanted_later_or_unwanted_ending_in_birth
         )%>%
  rename(counties_no_provider = percent_of_counties_without_a_known_abortion_provider,
         percent_abortion = percent_of_pregnancies_ending_in_abortion,
         percent_birth = percent_of_pregnancies_ending_in_birth, 
         percent_medicaid = percent_of_women_aged_15_44_covered_by_medicaid,
         percent_women_no_provider = percent_of_women_aged_15_44_living_in_a_county_without_an_abortion_provider, 
         percent_uninsured = percent_of_women_aged_15_44_who_are_uninsured,
         percent_bc_18_49 = percent_of_women_aged_18_49_using_contraceptives,
         abortion_rate_15_17_state = abortion_rate_the_no_of_abortions_per_1_000_women_aged_15_17_by_state_of_residence,
         abortion_rate_15_19_state = abortion_rate_the_no_of_abortions_per_1_000_women_aged_15_19_by_state_of_residence,
         abortion_rate_15_44_state = abortion_rate_the_no_of_abortions_per_1_000_women_aged_15_44_by_state_of_residence,
         abortion_rate_18_19_state = abortion_rate_the_no_of_abortions_per_1_000_women_aged_18_19_by_state_of_residence,
         birthrate_15_17_state = birthrate_the_no_of_births_per_1_000_women_aged_15_17_by_state_of_residence,
         birthrate_15_19_state = birthrate_the_no_of_births_per_1_000_women_aged_15_19_by_state_of_residence,
         birthrate_18_19_state = birthrate_the_no_of_births_per_1_000_women_aged_18_19_by_state_of_residence,
         need_bc_hisp_20_44 = no_of_women_who_likely_need_public_support_for_contraceptive_services_and_supplies_hispanic_aged_20_44_and_below_250_percent_of_the_federal_poverty_level,
         need_bc_hisp_younger_20 = no_of_women_who_likely_need_public_support_for_contraceptive_services_and_supplies_hispanic_younger_than_20,
         need_bc_13_44 = no_women_need_public_expenditure,
         need_bc_black_20_44 = no_of_women_who_likely_need_public_support_for_contraceptive_services_and_supplies_non_hispanic_black_aged_20_44_and_below_250_percent_of_the_federal_poverty_level,
         need_bc_black_under_20 = no_of_women_who_likely_need_public_support_for_contraceptive_services_and_supplies_non_hispanic_black_younger_than_20, 
         need_bc_white_20_44 = no_of_women_who_likely_need_public_support_for_contraceptive_services_and_supplies_non_hispanic_white_aged_20_44_and_below_250_percent_of_the_federal_poverty_level,
         need_bc_white_under_20 = no_of_women_who_likely_need_public_support_for_contraceptive_services_and_supplies_non_hispanic_white_younger_than_20, 
         need_bc_under_20 = no_of_women_who_likely_need_public_support_for_contraceptive_services_and_supplies_younger_than_20,
         total_expend_abortion = total_reported_public_expenditures_for_abortions_in_000s_of_dollars
         )%>%
    separate('abortion_rate_15_19_state',c(NA, 'abortion_rate_15_19_state'), sep=",")%>%
    separate('abortion_rate_15_44_state',c(NA, 'abortion_rate_15_44_state'), sep=",")%>% 
  separate('birthrate_15_19_state',c(NA, 'birthrate_15_19_state'), sep=",")%>%
    mutate(
    abortion_rate_15_19_state = str_replace_all(abortion_rate_15_19_state, "\\)", ""),
    abortion_rate_15_44_state = str_replace_all(abortion_rate_15_44_state, "\\)", ""),
    birthrate_15_19_state = str_replace_all(birthrate_15_19_state, "\\)", "")
    )%>%
    mutate(
    abortion_rate_15_19_state = as.numeric(abortion_rate_15_19_state),
    abortion_rate_15_44_state = as.numeric(abortion_rate_15_44_state),
    birthrate_15_19_state = as.numeric(birthrate_15_19_state),
    counties_no_provider = as.numeric(counties_no_provider),
    percent_abortion = as.numeric(percent_abortion),
   percent_birth = as.numeric(percent_birth),
    percent_medicaid = as.numeric(percent_medicaid),
    percent_women_no_provider = as.numeric(percent_women_no_provider),
    percent_uninsured = as.numeric(percent_uninsured),
    percent_bc_18_49 = as.numeric(percent_bc_18_49),
    abortion_rate_15_17_state = as.numeric(abortion_rate_15_17_state),
    abortion_rate_15_19_state = as.numeric(abortion_rate_15_19_state),
    abortion_rate_18_19_state = as.numeric(abortion_rate_18_19_state),
    birthrate_15_17_state = as.numeric(birthrate_15_17_state),
     birthrate_18_19_state = as.numeric( birthrate_18_19_state),
    need_bc_hisp_20_44 = as.numeric(need_bc_hisp_20_44),
    need_bc_hisp_younger_20 = as.numeric(need_bc_hisp_younger_20),
     need_bc_black_20_44 = as.numeric( need_bc_black_20_44),
    need_bc_black_under_20 = as.numeric(need_bc_black_under_20),
   need_bc_white_20_44 = as.numeric(need_bc_white_20_44),
    need_bc_white_under_20 = as.numeric(need_bc_white_under_20),
    need_bc_under_20  = as.numeric(need_bc_under_20),
   total_expend_abortion  = as.numeric(total_expend_abortion)
    )
```
