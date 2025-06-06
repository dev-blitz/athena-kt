# Remittance Batch File [ANSI-835] Components and Batch Process

1. First a `Claim` will be submitted with following details:

    1. Illness
    2. Procedure they took to cope with the Illness(Procedure Code)
    3. Amount

2. For Each Illness we have `ICD` codes (Disease Code):
    * ICD-9
    * ICD-10

3. These Codes are called as ***Procedure*** codes. *This code will represent the type of Illness the*
patient is getting treated for.

4. There are another type of Codes:
 CPD Codes (Procedure Code)

5. **2 things will received from the payers (Insurence Company):**

   1. Actual Amount
   2. Explanation for each and every *Claim*(List of ICD Codes and Procedure Code)

## ***Remittance***

   *It is the Explanation from the Insurance Company once they have paid the Claim or Group of Claims.*

## *Claims*

* *Claim* will have the folowing *details*:

  * Doctor's Name
  * Disease Code
  * Procedure Code for the Treatment Procedure

* *The **Claim** will be ***submitted*** by the **Hospital** and the Payment will be ***received*** by the Hospital*

* Claims are ***Grouped*** together and paid as a single Payment.
 We are doing it with ***Batch Processing***

* *Remittance* can be of 2 types:

 1. Paper
 2. `ANSI-835` Electronic Format:
    * Standard format for any Remittance related transaction between 2 Entities. It can be Hospital or middle man like Athena.
 3. `ANSI-837`:
    * Used for submitting the claims.

## ***Importing*** to ANSI

* We have to get the ANSI-835 Files, validate them and persist those in our Database.
* Each of them are submitting the ANSI-835 files differently.

## **ANSI-835** *files structure breakdown*

1. Always a File will start with `ISA`
2. The file will end with `IEA`
3. `GS` & `GR` represent groups.
4. `BPR` represents a Batch:
   `Batch` will have a group of `Claims`.
   The `Claims` will have a group of `Charges`.
   All these are paid together.
5. `BPR` represents an Entity of a `Payment`:
   It can be ***CHK*** (Cheque Payment) or ***ACH*** (Automated Clearing House) Payment.
6. Threre can be multiple *BPR* (Batched) in a File.
7. Each Batch will have a group of Charges inside them.
8. `BPR` attribute contains all the Claims till we get a next BPR.
9. `CLP` represents Claim:
  Until we get another **CLP** (*Claim*), the details entered refers to the first Claim.
10. `NM1` specifies some information about the claim:
   if `NM1` has *QC*, then it represents a `Patient`.
   `NM1` with *82* represents the `Doctor`.
11. `SVC` represents *Amount* each and every procedure for the claim that was taken.
  `SVC` represents Service Level Charges.
   `SVC` will contain the `Procedure Code` after '*'.
12. `CAS` represents the `Kick Codes` with the `Kick Amount`.
    * *CO*, *OA* are kick codes.
13. `PLB`:
    * The `Batch` amount may not be always equal to the umber of total amounts, there can be some additional amount paid or some amount may be reduced.
    * Any payment which is not specific to the number of claims will come as PLBs, which we are calling as Batch Exceptions.
    * These contain `Reason Codes` like WU, L6 which are used to find out the reason.

## **`SUMMARY`**

***Batch*** can have *group of **Claims `CLP`***, Each Claim will have *group of **Charges***. Other than Claims, it can also contain **Batch Exceptions `PLB`**.

***Batch*** will *start with* **[ISA]** and *end with* **[IEA]**
