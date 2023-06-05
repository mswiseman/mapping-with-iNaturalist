```r
library(rinat)

# A function to call `get_inat_obs` iteratively for each taxon_id present in a vector
batch_get_inat_obs <- function(
  taxon_ids_vector,            # No Default, so required, a vector of taxids
  place_id = 97394,            # 97394 is North America 
  geo = TRUE,                  # Default to TRUE
  maxresults = 1000,           # Default to 1000 results max
  meta = FALSE                 # Default to FALSE
) {
  
  # The dataframe (empty now) which we will return at the end, once it's full of results
  return_df = data.frame()
  
  # Iterate over the taxon ids in `taxon_ids_vector`, querying each again inaturalist 
  for (tid in taxon_ids_vector) {
    
    #Report status to console
    print(paste("Querying the following taxon id now:", tid))
    
    #Query inat here, using this taxon_id (`tid`, this iteration), and the other argument values
    #from the function declaration
    this_query = get_inat_obs(taxon_id = tid, 
                              place_id = place_id,
                              geo = geo,
                              maxresults = maxresults,
                              meta = meta)
    
    #Add the result of `get_inat_obs` on this iteration to a growing dataframe
    return_df = rbind(return_df, this_query)
    
    # Pause for 2 seconds
    Sys.sleep(2)
  }
  
  #After the loop is done, return the full dataframe
  return(return_df)
}

# The vector of taxon_ids (broken into three so we don't do too many requests at a time)
tids1 = c(58689, 62475, 62476, 62477, 62478, 118136, 118137, 118143, 120276, 120278, 123705, 125191, 125192, 152592, 175185, 175504, 175666, 175719, 175781, 175806, 175807, 175838, 175846, 175948, 175955, 176008, 194078, 194079, 194496, 194497, 214843, 216799, 216800, 218289, 220295, 220296, 220297, 223560, 230003, 230011, 321578, 335926, 335928, 337966, 338571, 338572, 350719, 350721, 350838, 350840, 351024, 351034, 351184, 351203, 351397, 351513, 351516, 352248, 352357, 352560)

tids2 = c(352628, 352630, 352631, 352632, 352636, 352638, 353107, 353130, 353143, 374417, 375161, 375318, 382686, 384118, 384119, 384120, 384121, 415193, 417573, 417992, 473934, 474681, 498441, 499218, 499752, 500001, 500022, 500138, 500178, 505028, 505029, 505041, 505043, 505048, 505091, 505352, 517784, 520327, 537568, 548621, 548622, 577699, 629108, 629233, 703365, 703368, 703371, 740810, 794018, 818601, 829601, 846168, 846169, 854513, 855941, 868663, 871576, 875053, 890164, 891028, 959235, 980435)

tids3 = c(980458, 987500, 1019026, 1021236, 1021248, 1023540, 1027280, 1027281, 1030401, 1042129, 1042231, 1043369, 1044907, 1047204, 1048279, 1069005, 1069006, 1069014)

tids4 = c(1096034, 1096038, 1096040, 1096058, 1096087, 1096093, 1096096, 1104372, 1110670, 1130869, 1131176, 1146657, 1148623, 1151995, 1153326, 1176632, 1180672, 1180799, 1225438, 1248133, 1260261, 1266404, 1302989, 1303014, 1303016, 1303018, 1325112, 1348171, 1370582, 1378034, 1396198, 1414676, 1468890, 1470944)

# Call the function and assign the result to nats[x]
nats1 = batch_get_inat_obs(tids1)
nats2 = batch_get_inat_obs(tids2)
nats3 = batch_get_inat_obs(tids3)
nats4 = batch_get_inat_obs(tids4)

# had to do in chunks to prevent overwhelming server
north_american_truffles <- rbind(nats1, nats2, nats3, nats4)
```
