# SQL-Assignment
#6-92a
select AgreementNbr, AgreementDate, GrossAmount
from AGREEMENT_T
where GrossAmount >= 1500 and GrossAmount <= 2000;

#6-92b
select CustomerID,CustomerName
from CUSTOMER_T
where CustomerName like '%Arts%';

#6-92c
select ArtistID, LastName, FirstName, YearOfBirth, ArtistType, Gender, State
from ARTIST_T
where Gender = 'F' and State in ('PA','NJ');

#6-92d
select ArtistID, StartDate, EndDate, RoyaltyPerc
from CONTRACT_T
where RoyaltyPerc > 20.00 and
CONTRACT_T.EndDate>= curdate()
order by ArtistID ASC;

#6-92e
select sum(Amount) as TotalAmount
from CUSTOMERPAYMENT_T
where CPaymentDate between '2015-02-01' and '2015-03-31';

#6-92f
select ArtistID
from ARTISTPAYMENT_T
where APaymentDate between '2015-01-01' and '2015-03-31'
group by ArtistID
having sum(Amount) >= 2000.00 ;

#6-92g
select count(EventID) as EventNbr, VenueID
from EVENT_T
group by VenueID;

#6-92h
select count(EventID) as EventNbr, VenueID
from EVENT_T
group by VenueID
having EventNbr >= 2;

#6-92i 
select ExpenseID, Description, Amount, ExpenseType
from EXPENSE_T
where (ExpenseType = "A" and Amount <= 50) 
or (ExpenseType = "M" and Amount <= 100); 

#7-77a
select distinct ARTIST_T.ArtistID, 
concat(ARTIST_T.FirstName,' ',ARTIST_T.LastName) as Fullname, YearOfBirth, RoyaltyPerc
from CONTRACT_T, ARTIST_T
where CONTRACT_T.ArtistID = ARTIST_T.ArtistID and
CONTRACT_T.EndDate>= curdate();

#7-77b
select distinct EVENT_T.EventID, EventDesc, DateTime
from EVENT_T, ARTIST_T, AGREEMENT_T, CONTRACT_T
where EVENT_T.EventID = AGREEMENT_T.EventID and
AGREEMENT_T.ContractID = CONTRACT_T.ContractID and
CONTRACT_T.ArtistID = ARTIST_T.ArtistID and
FirstName = 'Juan' and LastName = 'Becker';

#7-77c
select FirstName, LastName, EVENT_T.EventID, EventDesc, DateTime, GrossAmount
from EVENT_T, AGREEMENT_T, CONTRACTEDARTIST_T, CONTRACT_T, ARTIST_T
where EVENT_T.EventID = AGREEMENT_T.EventID and
CONTRACTEDARTIST_T.ArtistID = CONTRACT_T.ArtistID and
CONTRACT_T.ContractID = AGREEMENT_T.ContractID and
ARTIST_T.ArtistID = CONTRACT_T.ArtistID and
AManagerID = '1' and 
DateTime between '2014-12-01' and '2015-01-31';

#7-77d 
select distinct FirstName, LastName, EVENT_T.EventID, EventDesc, DateTime, GrossAmount, 
round(GrossAmount*(1-RoyaltyPerc/100),2) as AShare,
round((GrossAmount*RoyaltyPerc/100*0.5),2) as AMshare, 
round((GrossAmount*RoyaltyPerc/100*0.5),2) as FAMEshare
from AGREEMENT_T, ARTISTPAYMENT_T, CONTRACT_T, EVENT_T, CONTRACTEDARTIST_T, ARTIST_T
where AGREEMENT_T.ContractID = CONTRACT_T.ContractID and
AGREEMENT_T.EventID = EVENT_T.EventID and
CONTRACT_T.ArtistID = ARTISTPAYMENT_T.ArtistID and
CONTRACTEDARTIST_T.ArtistID = ARTISTPAYMENT_T.ArtistID and
ARTIST_T.ArtistID = CONTRACT_T.ArtistID and
AManagerID = '1'and 
DateTime between '2014-12-01' and '2015-01-31';


#7-77e 
select distinct EVENT_T.EventID, EventDesc, DateTime, GrossAmount as Revenue, 
round(GrossAmount*(1-RoyaltyPerc/100),2) as AShare
from EVENT_T, AGREEMENT_T, ARTISTPAYMENT_T, CONTRACT_T, ARTIST_T
where EVENT_T.EventID = AGREEMENT_T.EventID and
ARTISTPAYMENT_T.ArtistID = CONTRACT_T.ArtistID and
CONTRACT_T.ContractID = AGREEMENT_T.ContractID and
ARTIST_T.ArtistID = CONTRACT_T.ArtistID and
FirstName = 'Juan' and LastName = 'Becker' and
Datetime between '2014-12-01' and '2015-03-31';

#7-77f
select PERFORMANCERELATEDC_T.ACommitmentID, PRCCategory, EventDesc, 
StartDateTime, EndDateTime, ARTIST_T.ArtistID, CommitmentType
from EVENT_T, PERFORMANCERELATEDC_T,  ARTISTCOMMITMENT_T, ARTIST_T
where EVENT_T.EventID = PERFORMANCERELATEDC_T.EventID and 
ARTIST_T.ArtistID = ARTISTCOMMITMENT_T.ArtistID and
ARTISTCOMMITMENT_T.ACommitmentID = PERFORMANCERELATEDC_T.ACommitmentID and
FirstName = 'Pat' and LastName = 'Jiminez' and 
Datetime between '2014-12-01' and '2014-12-31';

select PERFORMANCERELATEDC_T.ACommitmentID, PRCCategory, EventDesc, 
StartDateTime, EndDateTime, ARTIST_T.ArtistID, CommitmentType
from EVENT_T 
left outer join PERFORMANCERELATEDC_T
left outer join ARTISTCOMMITMENT_T
left outer join ARTIST_T
where EVENT_T.EventID = PERFORMANCERELATEDC_T.EventID and 
ARTIST_T.ArtistID = ARTISTCOMMITMENT_T.ArtistID and
ARTISTCOMMITMENT_T.ACommitmentID = PERFORMANCERELATEDC_T.ACommitmentID and
FirstName = 'Pat' and LastName = 'Jiminez' and 
Datetime between '2014-12-01' and '2014-12-31';

select PRCCategory,EventDesc,ARTISTCOMMITMENT_T.ACommitmentID,ARTISTCOMMITMENT_T.StartDateTime,
ARTISTCOMMITMENT_T.EndDateTime,ARTISTCOMMITMENT_T.ArtistID,ARTISTCOMMITMENT_T.CommitmentType
from EVENT_T
right outer join PERFORMANCERELATEDC_T on PERFORMANCERELATEDC_T.EventID = EVENT_T.EventID
right outer join ARTISTCOMMITMENT_T on ARTISTCOMMITMENT_T.ACommitmentID = PERFORMANCERELATEDC_T.ACommitmentID
where ARTISTCOMMITMENT_T.ArtistID=10 and EVENT_T.DateTime between "2014-12-01" and "2014-12-31";
