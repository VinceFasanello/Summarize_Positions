# ETF_Summarize_Positions
 Summarize account positions to explore holdings overlap among all ETFs and individually owned stocks.

Instructions:

	[A] instructions to RUN:
		(1) install software (see "[B]" below)
		(2) prepare input files
		(3) open Summarize_Positions.Rproj
		(4) if (Calculate.R does not open automatically in Rstudio):
			- go to the File menu, click Open File..., navigate to the Calculate.R file, open.
		    else:
		(5) run Calculate.R in its entirety. (highlight all the code and hit ctrl+enter, cmd+enter on mac)
		(6) a decomposedPositions file will be produced in the Summarize_Positions directory   
		

	[B] Software Requirements:
		(1) R
		(2) RStudio
		(3) R Package: "here" (uncomment line 8 in Calculate.R to auto-install)
	
	[C] Input Files:
		(1) Portfolio_Positions.csv 
			Description: portfolio positions for the account to be analyzed
			Column 1: account position symbols (e.g., AAPL)
			Column 2: entry descriptions (e.g., APPLE INC); NA or empty cell if no description 
			Column 3: boolean: TRUE if the entry is an ETF (or mutual fund), FALSE otherwise
			Column 4: country for stocks (e.g., United States), NA if ETF (or mutual fund), as desired for MISC things (e.g., Gold, Core Position)
			Column 5: current value for position in desired currency (e.g., USD)
		
		(2) ETF holdings csv's (one CSV for each ETF in the Portfolio_Positions.csv input)
			Description: basket holdings for individual ETFs (download from your favorite online source, I used fidelity.com)
			Column 1: account position symbols (e.g., AAPL); OK if some of these are blank, they will be grouped as MISC by the Calculate.R script.
			Column 2: entry descriptions (e.g., APPLE INC); NA or empty cell if no description 
			Column 3: country; NA or empty cell if no country
			Column 4: Weight -- the relative proportion of the position (row) in that etf (file); OK if these do not add to one, this is handled by the Calculate.R script.


	[D] Directory & File Architechture:
		- Summarize_Positions (directory)
			- Portfolio_Positions.csv
			- ETF_CSVs (directory)
				- ETF_ETFSYMBOL1.csv
				- ETF_ETFSYMBOL2.csv
				- Etc.....
			- Summarize_Positions.Rproj
			- Calculate.R
			- README.md
			
