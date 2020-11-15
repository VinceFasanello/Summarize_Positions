# Dislaimer: The read.csv and write.csv commands in this script will only work with the "here" package 
# if this code is opened as an R project in RStudio. If you got here some other way you are wrong....go back 
# to the repository files you downloaded and open the Summarize_Positions.Rproj file in RStudio. If this 
# file (Calculate.R) does not open automatically, open it from within the R Project (i.e., go to file --> Open File...).


# Top Matter -------------------------------------------------------------------
# install.packages("here") # uncomment this to install the "here" package used for directory management.
require(here) # load PACKAGE: here


# Prep accountPositions data ---------------------------------------------------
accountPositions <- read.csv(here("Portfolio_Positions.csv")) # get account positions
colnames(accountPositions) <- c("symbol", "description", "isETF", "country", "cv") # standardize column names
accountPositions$cvWeight <- accountPositions$cv / sum(accountPositions$cv, na.rm = T) # calculate current value weights for positions in ACCOUNT
accountTotalValue <- sum(accountPositions$cv) # calculate the total currentValue of the account
etfSymbols <- accountPositions$symbol[accountPositions$isETF == T] # get a list of the symbols in the positions dataset that are ETFs

# Read and concatonate ETF holdings --------------------------------------------
decomposedPositions <- NA # seed for dataframe that is built in following loop.
for (i in etfSymbols) { # for each etf in the account...
  temp <- read.csv(here("ETF_CSVs", paste0("ETF_", i, ".csv"))) # load the holdings csv for that ETF
  colnames(temp) <- c("symbol", "description", "country", "weight") # standardize column names
  if(length(temp[is.na(temp$symbol) | temp$symbol == "",]$symbol) > 0){ # if there are any strange holdings...
    temp[is.na(temp$symbol) | temp$symbol == "",]$symbol <- "MISCMINORHOLDING" # ...add to the MISC bag.
  }
  temp$weight <- temp$weight/sum(temp$weight) # HOLDING weights sum to 1 within each ETF.
  temp$cvWeight <- NA # cvWeight = weight of ETF in ACCOUNT * weight of HOLDING in ETF 
  temp$cvWeight <- temp$weight*accountPositions[accountPositions$symbol == i,]$cvWeight
  decomposedPositions <- rbind(decomposedPositions, temp) # grow the decomposedPositions dataframe with this ETF's data...and loop.
}; rm(temp, i, etfSymbols)

# Summarize ETF and other positions to one entry per unique symbol -------------
decomposedPositions <- decomposedPositions[2:nrow(decomposedPositions),c(1:3,5)] # retain only rows and columns of interest
decomposedPositions <- rbind(accountPositions[accountPositions$isETF == F, c(1,2,4,6)],
                             decomposedPositions); rm(accountPositions) # bind the non-ETF and ETF data.
uniqueSymbols <- unique(decomposedPositions$symbol) # get a list of unique ticker symbols
temp <- data.frame(matrix(NA, ncol = 4, nrow = length(uniqueSymbols))) # create a dataframe to hold our final output.
colnames(temp) <- colnames(decomposedPositions) # assign column names to the output frame
temp$symbol <- uniqueSymbols # assign data to the symbol column of the output frame
for (i in uniqueSymbols) { # for each unique symbol...
  temp2 <- decomposedPositions[decomposedPositions$symbol == i,] # get all entries for that symbol
  temp[temp$symbol == i,]$description <- temp2$description[1] # just keep the first description (they are all the same)
  temp[temp$symbol == i,]$country <- temp2$country[1] # just keep the first country (they are all the same)
  temp[temp$symbol == i,]$cvWeight <- sum(temp2$cvWeight) # sum the weights to get the total cvWeight for that symbol...and loop.
}; rm(temp2, i, uniqueSymbols)

# clean up the resulting dataframe ---------------------------------------------
temp[temp$symbol == "MISCMINORHOLDING", c(2:3)] <- "MISCMINORHOLDING" # fix the info for the MISC entry.
temp$currentValue <- temp$cvWeight*accountTotalValue; rm(accountTotalValue) # calculate the currentValue for each position 
temp$symbol <- as.factor(temp$symbol) # fix some factors.
temp$country <- as.factor(temp$country)
colnames(temp)[4] <- "propOfAccount" # rename the cvWeight column to be more readable.
temp <- temp[order(temp$currentValue, decreasing = T),] # sort in descending order by currentValue
rownames(temp) <- seq(1, nrow(temp))
decomposedPositions <- temp; rm(temp) # assign to the appropriate output name.

# output the decomposedPositions CSV -------------------------------------------
write.csv(decomposedPositions, file = here("decomposedPositions.csv"), row.names = F) # save the output!
