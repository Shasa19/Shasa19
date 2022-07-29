1. Dividend-adjusted returns are calculated using the dividend adjusted prices, from the Prices of all companies file, with the formula: 
((price on the current date - price on the previous date)/price on the previous date)

2. ESG scores, from the 1. ESG data file, and returns are put together in one file so the company has the correct ESG score and return per day. This has been 
done manually by copying the ESG score of a certain compamy next to the returns of that company and then fill the the ESG score until the end of the year (as the ESG scores
are yearly). This process is repeated for all the pillars, for every year and all the companies.

3. Portfolios are sorted manually as well in excel. The ESG scores are ranked from high to low with the excel sort function in the 1. The scores are then divided in 
4 equal portfolios to determine what the cutoff scores are for each portfolios. Then the returns from all the companies that belong to that specific portfolio based on the
ESG score are selected from the prices file and copied next to each other in a seperate file in a time-series format. In this file the company returns are used to calculate 
the daily portfolio returns. The daily portfolio returns are calculated by adding up the daily returns from all the companies in that portfolio and dividing that by the number of companies 
in that portfolio (as they are equally weighted portfolios).

4. Once the portfolio returns are obtained the factors are also imported into the same file so all the factors match the correct date. Then the new file contains the dates
 with factors and the portfolio returns. Four seperate files are made for the pillars and for the overall ESG score, that contain the appropriate portfolio returns.

5. These files containing the factors and portfolio returns are imported in stata, to perform the OLS regressions (with standard and Newey West standard errors), 
heteroskedasticity test and autocorrelation test is performed in Stata. All the steps that are performed in Stata are stated underneath step 7 in this file and in the
Do-file that is attached in the zip file. The Q4,Q3,Q2 and Q1 Excess returns, that are stated in the stata code, refer to the Top, Follower, Lounger and Bottom 
portfolios's returns. The OLS regressions are repeated for every pillar and over different time periods aswell, this is also the case for the heteroskedasticity and
autocorrelation test.

6. The Sharpe ratio is performed in Excel. Firstly, the standard deviation is calculated using the following command =STDEV(column with returns) 
and mean return of the portfolio is calculated, using the =Average(column with returns) command.
The mean return is divided by the standard deviation to calculate the Sharpe ratio. Secondly, the Sharpe ratio standard error is calculated with the formula: 
=SQRT((1+1/2*daily sharpe ratio^2)/ number of observations. Subsequently, a two-paired t-test is performed, the formula is: 
(The Daily sharpe ratio - the SR target) / Sharpe ratio standard error), where the sharpe ratio target is zero. The test statistic is transformed
into the p-value by the excel formula: =T.DIST.2T(t-stat of the sharpe ratio, number of observations of returns -2) (where 
minus 2 is the degree of freedom.

7. Lastly, the maximum drawdown is calculated in excel. Firstly, the first portfolio price is calculated with the formula:
100*(1+daily return) from the second price on the cumulatitive price is calculated as: price from the previous day * (1+return of this day).
Then the drawdown is calculated, with the formula: 
=MIN((Portfolio price at that day- MAX(first day price:current day price))/MAX(First day price:current day price),0).
The Maximum drawdown is calculated with the formula: =MIN(column with drawdown:column with drawdown)

OLS Regressions

cd "\\cfs\users\skfd1\Documents\Diss stata"
import excel "\\cfs\users\skfd1\Documents\Diss stata\ESG excess overall portfolio returns.xlsx", sheet("Sheet1") firstrow
tsset Date, delta(1)
regress Q4Excessreturn
regress Q3Excessreturn
regress Q2Excessreturn
regress Q1Excessreturn
regress Q4Q1longshort
regress Q4Excessreturns MktRF3
regress Q3Excessreturns MktRF3
regress Q2Excessreturns MktRF3
regress Q1Excessreturns MktRF3
regress Q4Q1longshort MktRF3 
regress Q4Excessreturns MktRF3 SMB3 HML3
regress Q3Excessreturns MktRF3 SMB3 HML3
regress Q2Excessreturns MktRF3 SMB3 HML3
regress Q1Excessreturns MktRF3 SMB3 HML3
regress Q4Q1longshort MktRF3 SMB3 HML3
regress Q4Excessreturns MOM4  MktRF3 SMB3 HML3
regress Q3Excessreturns MOM4  MktRF3 SMB3 HML3
regress Q2Excessreturns MOM4  MktRF3 SMB3 HML3
regress Q1Excessreturns MOM4  MktRF3 SMB3 HML3
regress Q4Q1longshort MOM4 MktRF3 SMB3 HML3
regress Q4Excessreturns MktRF3 SMB3 HML3 RMW5 CMA5
regress Q3Excessreturns MktRF3 SMB3 HML3 RMW5 CMA5
regress Q2Excessreturns MktRF3 SMB3 HML3 RMW5 CMA5
regress Q1Excessreturns MktRF3 SMB3 HML3 RMW5 CMA5
regress Q4Q1longshort MktRF3 SMB3 HML3 RMW5 CMA5

heteroscedasticity test (for all)
estat hettest

" Repeat this for all pillars and for different periods"

Regressions with robust standard errors 

regress Q4excessreturns, vce(robust)
regress Q3excessreturns, vce(robust)
regress Q2excessreturns, vce(robust)
regress Q1excessreturns, vce(robust)
regress Q4Q1longshort, vce(robust)
regress Q4excessreturns MktRF3, vce(robust)
regress Q3excessreturns MktRF3, vce(robust)
regress Q2excessreturns MktRF3, vce(robust)
regress Q1excessreturns MktRF3, vce(robust)
regress Q4Q1longshort MktRF3, vce(robust)
regress Q4excessreturns SMB3 HML3 MktRF3, vce(robust)
regress Q3excessreturns SMB3 HML3 MktRF3, vce(robust)
regress Q2excessreturns SMB3 HML3 MktRF3, vce(robust)
regress Q1excessreturns SMB3 HML3 MktRF3, vce(robust)
regress Q4Q1longshort SMB3 HML3 MktRF3, vce(robust)
regress Q4excessreturns MOM4 SMB3 HML3 MktRF3, vce(robust)
regress Q3excessreturns MOM4 SMB3 HML3 MktRF3, vce(robust)
regress Q2excessreturns MOM4 SMB3 HML3 MktRF3, vce(robust)
regress Q1excessreturns MOM4 SMB3 HML3 MktRF3, vce(robust)
regress Q4Q1longshort MOM4 SMB3 HML3 MktRF3, vce(robust)
regress Q4excessreturns RMW5 CMA5 SMB3 HML3 MktRF3, vce(robust)
regress Q3excessreturns RMW5 CMA5 SMB3 HML3 MktRF3, vce(robust)
regress Q2excessreturns RMW5 CMA5 SMB3 HML3 MktRF3, vce(robust)
regress Q1excessreturns RMW5 CMA5 SMB3 HML3 MktRF3, vce(robust)
estat hettest
" Repeat this for all pillars and for different periods"
Autocorrelation test for each pillar, portfolio and period

tsset Date, delta(1)
estat bgodfrey, lag(1)

Regression with Newey West standard errors
import excel "\\cfs\users\skfd1\Documents\Diss stata\ESG excess returns final adj.xlsx", sheet("Sheet1") firstrow
tsset Date, delta(1)
newey Q4excessreturns MktRF3 SMB3 HML3 RMW5 CMA5, lag(1)
newey Q3excessreturns MktRF3 SMB3 HML3 RMW5 CMA5, lag(1)
newey Q2excessreturns MktRF3 SMB3 HML3 RMW5 CMA5, lag(1)
newey Q1excessreturns MktRF3 SMB3 HML3 RMW5 CMA5, lag(1)
newey Q4Q1longshort MktRF3 SMB3 HML3 RMW5 CMA5, lag(1)
newey Q4excessreturns MktRF3 SMB3 HML3 MOM4, lag(1)
newey Q3excessreturns MktRF3 SMB3 HML3 MOM4, lag(1)
newey Q2excessreturns MktRF3 SMB3 HML3 MOM4, lag(1)
newey Q1excessreturns MktRF3 SMB3 HML3 MOM4, lag(1)
newey Q4Q1longshort MktRF3 SMB3 HML3 MOM4, lag(1)
newey Q4excessreturns MktRF3 SMB3 HML3, lag(1)
newey Q3excessreturns MktRF3 SMB3 HML3, lag(1)
newey Q2excessreturns MktRF3 SMB3 HML3, lag(1)
newey Q1excessreturns MktRF3 SMB3 HML3, lag(1)
newey Q4Q1longshort MktRF3 SMB3 HML3, lag(1)
newey Q4excessreturns MktRF3, lag(1)
newey Q3excessreturns MktRF3, lag(1)
newey Q2excessreturns MktRF3, lag(1)
newey Q1excessreturns MktRF3, lag(1)
newey Q4Q1longshort MktRF3, lag(1)
newey Q4excessreturns, lag(1)
newey Q3excessreturns, lag(1)
newey Q2excessreturns, lag(1)
newey Q1excessreturns, lag(1)
newey Q4Q1longshort, lag(1)


Fama Macbeth

import excel "\\cfs\users\skfd1\Documents\Diss stata\1 Panel data ESG overall - Copy.xlsx", sheet("ESG panel") firstrow
xtset Date ID_Num
ssc install asreg
asreg ExcessReturn MktRF3, fmb
asreg ExcessReturn MktRF3 SMB3 HML3, fmb
asreg ExcessReturn MktRF3 SMB3 HML3 MOM4, fmb
asreg ExcessReturn MktRF3 SMB3 HML3 RMW5 CMA5, fmb

