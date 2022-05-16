## Team Name
Quatre.

## Job Allocation
- **Tserundede Godswill**: Project administrator/Media.
- **Isaac Jesse**: Project Coordinator, Developer - Frontend/Backend/(Solidity, Web3, Reactjs, NextJs).
- **Shewa** : Designer.
- **Rex**: Frontend.
- **Gbenga**: UI/UX

## Project Intro
Quatre-finance is a platform for decentralized applications. We aim not to completely uninstall the application world but to continuously re-emphasize the need for true decentralization, while building decentralized products tailored to individual needs putting them in complete control of their funds and/or investments. [More information about Quatrefinance](https://github.com/Quatre-Finance/Q-paper/blob/main/INTRO.md). 

Quatrefinance as the umbrella/parent project, is building from but not limited to four major catgories: 

### Exchange.
### Lending and Borrowing.
### p2P.
### Wallet Service.
---------------------------
## Quatre Digesu
This is first ever product launched by Quatrefinance from the second category targeted at digitalizing a culture custom to the African communities. It introduces a new paradigm to an existing scheme of lending and borrowing culture of the Africans, remove the known barriers and make it accessible globally. Usually, it involves two or more people coming together to form a group for the purpose of raising funds by peer contribution, and does not involve paying interest.

Often, participations are limited to a close location/area or a set of people where one knows someone and knows another. Yet, the known dare devils has not been solved by a mere man knows man since trust is a destination that is practically unreachable where human to human interaction is concerned. Some of the existing problems with this scheme itemized as:

- **Trust issue**: Over the years, people are yet to overcome the recurring event of trust, hence, many are discouraged because they cannot entrust money in another. However, this accounts for why many were unable to gain financial freedom overtime.

- **Absconders**: Some of the participants would joined with the motive of parting away with others' hard earned money. 

- **Delay in payment**: There are myriad cases where participants delay and default in repayment of the loans.

Digesu users can interact in three ways: 

- **Finance**: Lending and borrowing without interest. We are also adding a feature where farmers and Artisans can have access to credit facilities.

- **Invest**: Users can commit idle funds or rather called 'savings' in Digesu's Public Permissionless Robot (PPR) to increase their income instead of keeping them idle and being eaten up gradually by inflation. Users can choose from investment options such as Stakings, Liquidity and StartUps.

- **Governance**: We believe Digesu community will be large, hence we created an incentivized Decentralized Autonomaus Community (DAC) to encourage effective participation in decision making.

-----------------------

### Second category and biz model

The second category of Digesu has a picture of what we hope to achieve even on a long-term. Starting from Nigeria, We will combine both web3 and web2 to bring blockchain to every doorsteps. 

Looking at the huge population of Nigeria (over 200 Million), from our research (not confirmed), at least 7% of the workforce do daily contribution (daily savings), of which most of them keep money with either micro finance, a man who comes around or someone from the yard, take one of every 'N' contributions they made i.e N/N ==> charges.

This category inherits similar setbacks from the first category. Our aim is to leverage blockchain solution to help people make the best use of their savings, give them rest of mind they desire, and secure their savings from inflation attack. 

Take for instance, we are able to onboard say 5% of the total population on our platform, a projected income would be:

Assume each user saves at least NGN100 daily for 7 days or 30 days. As an implied standard, the custodian fee is one of every 'N' contributions made irrespecive of whether they complete the period or not.

Assume a total popultion of 200 Million.

Target user (TU) = 5% * 200M ==> NGN1e7.
Average Daily Contribution (ADC) = NGN100.
where T = Taxation.

projected montly revenue (if weekly subscribers) = ADC * TU * 4 ==> NGN4e9 -T.
projected montly revenue (if monthly subscribers) = ADC * TU ==> NGN1e9 -T. 

-----------------------

## Our Motivation
The future of blockchain as the mother technology is bright, even though we are just starting. So we are excited to join the adoption movement by creating solutions that solves people's daily challenges.

-----------------------

## Digesu's Technology and Architeture

Digesu is designed to be blockchain agnostic i.e cross-chain compatibility with smart contract backing the protocol, a frontend, and design frames. 
Even though we have different repositiories for each of the frameworks, for the purpose of this documentation, we have compressed both the frontend and solidity code in one repository that can be found **[here](https://github.com/Quatre-Finance/Core)**.

The current frontend design for [Digesu](https://app.quatre.finance) is built for demo purpose. **[Here is the design we made for production](https://www.figma.com/file/eAK52TVb7n0HTwlNvtb4gv/?node-id=219%3A2655)**.

#### Overview of the SC

Putting security first, We have adopted four patterns in designing the smart contract:
 - **Factory pattern**.
 - **Proxy pattern**.
 - **Separation of concern or Parent-Child pattern**.
 - **Modular approach**.

The base contract `DexPoolOneFile` is modular, compressed into one file for easy verification. It is responsible for creating new instances of the proxy contract. Most of the calls to the `DexPoolOneFile` from the frontend results in contract creation. So, each created band or community is identified by a unique contract address. For instance, if Bob launched a new band to raise loan, in the backend, a seprate contract is created for this course. Subsequent interact from Bob and every other participants of the same community is done from the created address. The initialization is done using an existing module from the **[Openzeppelin library](https://github.com/OpenZeppelin/openzeppelin-contracts)**. Shoutout to the OZ guys who made it available for us to use. Using unique salt parameter during initialization gives us an assurance that two bands cannot coexist. Since each band is administered from an entirely different address location hence no single point of failure.

Aside of band creation utilities, other functional utilities in the **DexPoolOneFile** are administrative. We achieve this using a custom access moderating contract `Auth.sol`.

#### Pproxy.sol

This is the child contract responsible for adminstering functions such as:
  - **[getFinance()]()**: Locked on intialization. Active only when the quorum is achieved. Can be called by any of the participants or members but fund is send to the next in queue. This function will fail if: 
      - caller is not a member of the band.
      - caller is a member but does not have enough `QFT` in wallet to deposit as collacteral.
      - other conditions for access are not true.

  - **[payBack()]()**: Allows member (s) with outstanding debt to repay their loan. It is locked at construction. Act as condition for unlocking `getFinance` if call was successfull.

  - **[completeTheRound()]()**: Can be used to formally close the community only if everyone in the community is satistfied.

  - **[claim()]()**: After the round is closed, each member can withdraw or claim collateral balance. This is an explicit call, not automated.

  - **[liquidate()]()**: Liquidates the current beneficiary if they they have exceeded the repayment time given. It should be manually called by any of the members of the band. Request can only be executed if current timestamp is greater than expected repayment time.

  - **[absorb()]()**: Any of the members in a community can absorb the current debt if the user has defaulted in payment. When this happens, the defaulter's collateral balance is weighed, a 5% penalty is charged against the defaulter in favor of the authorized caller. The balance of which is rolled back to the pool, and sent to the next in queue.

It is unique to each band, perhaps group of participants.

-------------------

### How we generate revenue from this category

  Users pay a one-time fee (usually minimal) for creating a band. This is incurred by the band creator. In addition, the platform token (QFT) will be used as denomination for collateral. More information about this will be released later.

  -------------------

## The Frontend
  We have adopted two frameworks to built the frontend: 
  - NextJs, and
  - MaterialUI

`Note`: Please refer to the package.json for full dependencies and packages.

#### Deployment
Digesu currently runs on Conflux testnet with following deployment details

- **Deployer**: 0xe29D321FB47c03e3b3aab3fF1AB78b344F9De3cD
- **DexPool Contract Address**: 0x31C8dD6042819BA14e55418F9e810727fF21C67d
- **Test QFT**: 0x69a8a8181E9E43Ea5f125b796d359be50fdC4bFa
- **Initial Mint**:  ```{
  hash: '0xfc0677b859f36e5a8a86e150910d4de6ef485e2da07a39e65612b2621c5f2867',
  type: 0,
  accessList: null,
  blockHash: null,
  blockNumber: null,
  transactionIndex: null,
  confirmations: 0,
  from: '0xe29D321FB47c03e3b3aab3fF1AB78b344F9De3cD',
  gasPrice: BigNumber { _hex: '0x01', _isBigNumber: true },
  gasLimit: BigNumber { _hex: '0x04c34f', _isBigNumber: true },
  to: '0x69a8a8181E9E43Ea5f125b796d359be50fdC4bFa',
  value: BigNumber { _hex: '0x00', _isBigNumber: true },
  nonce: 7,
  data: '0xb6afc4dc000000000000000000000000000000000000000000000000000000000000004000000000000000000000000000000000000000000000001b1ae4d6e2ef5000000000000000000000000000000000000000000000000000000000000000000004000000000000000000000000e29d321fb47c03e3b3aab3ff1ab78b344f9de3cd0000000000000000000000009aeaf25960004b1cd5c1932ffd5c3a72713683a7000000000000000000000000e63537c16094fcd6b49fe6730c182ec973940f4f000000000000000000000000a7b2387bf4c259e188751b46859fca7e2043fefd',
  r: '0x8d2c9b2321855c218fcdfea7b9d7069d4684f376b5b1e9045d01a1ae5fde6bab',
  s: '0x774f6793dc7c43856553cd6a792505268762aad4c37e2266513df518284fef2e',
  v: 178,
  creates: null,
  chainId: 71,
  wait: [Function (anonymous)]
}```


We deployed the DApp using Vercel, running at the domain **[app.quatre.finance](https://app.quatre.finance)**.

---------------------------

**Video Link**: **[https://youtu.be/AzKNxFZxf18](https://youtu.be/AzKNxFZxf18)**

-----------------

**Repository Address**

**[https://github.com/Quatre-Finance/Core](https://github.com/Quatre-Finance/Core)**