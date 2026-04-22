# Database Collections Inventory

Overview of all MongoDB collections in the TaaS backend, organized by domain.

Branch: `version/CIMB-tokenized-deposits-finalised` (67 collections — 63 from dev + 4 TD-specific)

---

## Users & Authentication (10 collections)

| Collection | Model | Purpose |
|---|---|---|
| `users` | User | All user accounts with roles, KYC/KYB, bank accounts, approval status |
| `approvalrequests` | ApprovalRequest | Approval requests for account actions (create, activate, delete) |
| `roles` | Role | Base role definitions with permissions |
| `issuerroles` | IssuerRole | Issuer-specific role permissions |
| `custodianroles` | CustodianRole | Custodian-specific role permissions |
| `investorroles` | InvestorRole | Investor-specific role permissions |
| `primarysubscriberroles` | PrimarySubscriberRole | PS-specific role permissions |
| `facilityagentroles` | FacilityAgentRole | FA-specific role permissions |
| `trusteeroles` | TrusteeRole | Trustee-specific role permissions |
| `superadminroles` | SuperAdminRole | Super admin role permissions |

## Bonds & Securities (6)

| Collection | Model | Purpose |
|---|---|---|
| `bonds` | Bond | Bond issuance details (coupon, maturity, ISIN, token info, approval) |
| `bondredemptions` | BondRedemption | Maturity redemption approval workflow |
| `bondprimarysubscribers` | BondPrimarySubscriber | PS assignment to bonds |
| `bondtrustees` | BondTrustee | Trustee assignment to bonds |
| `bondfacilityagents` | BondFacilityAgent | FA assignment to bonds |
| `bondmarketprices` | BondMarketPrice | Market pricing data |

## Subscriptions & Allocations (6)

| Collection | Model | Purpose |
|---|---|---|
| `subscriptions` | Subscription | Subscription periods with allocation status |
| `allocations` | Allocation | Primary allocations per investor per bond |
| `custodianallocations` | CustodianAllocation | Custodian allocations to sub-investors |
| `allocationrequests` | AllocationRequest | Investor LOC requests (snapshot: `selectedWallet`). **TD fields**: `isSettleViaTokenizedDeposits`, `tdTypeId`, `tdTypeSnapshot`, `tidesSubmissionId/No`, `tidesTopupStatus/Amount` |
| `custodianbondallocationapprovals` | CustodianBondAllocationApproval | L1/L2 approval for custodian bond allocations |
| `locamountchangerequests` | LOCAmountChangeRequest | LOC amount change requests |

## Token & Transfers (4)

| Collection | Model | Purpose |
|---|---|---|
| `tokentransfers` | TokenTransfer | Token transfers with blockchain details |
| `secondarytransfers` | SecondaryTransfer | Secondary market trades between investors |
| `secondarytransferapprovals` | SecondaryTransferApproval | Approval workflow for secondary transfers |
| `bondtokenrequests` | BondTokenRequest | Cash-to-bond token swap requests (snapshot: `investorSnapshot.bankAccountDetails`). **TD fields**: `isSettleViaTokenizedDeposits`, `custodianSettlementType`, `tdTypeId`, `tdTypeSnapshot` |

## Holdings & Wallets (4)

| Collection | Model | Purpose |
|---|---|---|
| `holdings` | Holding | Bond holdings per investor with token tracking |
| `custodianholdingdistributions` | CustodianHoldingDistribution | Custodian distribution to sub-investors |
| `psholdingdistributions` | PsHoldingDistribution | PS distribution to allocees |
| `wallets` | Wallet | User wallets with bank details, network, AML status. **TD change**: `bankName` and `accountNumber` now `required: false` (TD wallets may not have bank details) |

## Tokenised Deposits (4) — NEW on TD branch

| Collection | Model | Purpose |
|---|---|---|
| `tdtypes` | TdType | Registry of tokenised deposit types (code, name, token contract address, provider, decimals) |
| `tdwithdrawalrequests` | TdWithdrawalRequest | TD withdrawal requests from issuer/PS wallets (TIdes integration, on-chain tx tracking, balance deduction flag) |
| `wallettdbalances` | WalletTdBalance | Per-wallet TD balance per TD type (Decimal128, unique on walletId+tdTypeId) |
| `wallettdtransactions` | WalletTdTransaction | Append-only ledger of TD balance movements (credit/debit amount, balanceAfter, transactionType, metadata) |

## Payments & Payouts (8)

| Collection | Model | Purpose |
|---|---|---|
| `payments` | Payment | Payment schedules (coupon/principal) |
| `custodianpayments` | CustodianPayment | Custodian internal payment processing |
| `custodiancouponpayments` | CustodianCouponPayment | Coupon distribution to sub-investors |
| `custodianmaturitypayments` | CustodianMaturityPayment | Maturity distribution to sub-investors |
| `redemptionpayments` | RedemptionPayment | Redemption payment confirmations |
| `redemptions` | Redemption | Maturity redemptions tracking |
| `maturitypaymentschedules` | MaturityPaymentSchedule | Maturity payment schedules |
| `payouts` / `maturitypayouts` | Payout / MaturityPayout | Payout records |

## Requests & Workflow Approvals (16)

| Collection | Model | Purpose |
|---|---|---|
| `subagreementrequests` | SubAgreementRequest | PS sub-agreement requests (snapshot: `selectedWallet`). **TD fields**: `isSettleViaTokenizedDeposits`, `tdTypeId`, `tdTypeSnapshot`, `tidesSubmissionId/No`, `tidesTopupStatus/Amount` |
| `subagreementtemplates` | SubAgreementTemplate | Sub-agreement document templates |
| `maturitydistributionrequests` | MaturityDistributionRequest | Maturity payment distribution requests |
| `coupondistributionrequests` | CouponDistributionRequest | Coupon payment distribution requests |
| `psinternalapprovals` | PSInternalApproval | Internal PS action approvals |
| `burnrequests` | BurnRequest | Bond token burn at maturity |
| `burncashtokenrequests` | BurnCashTokenRequest | Cash token burn requests |
| `allocationapprovals` | AllocationApproval | General allocation approval workflow |
| `custodianallocationapprovals` | CustodianAllocationApproval | Custodian allocation approval (snapshot: `l1Payload.allocations[].bankAccountDetails`) |
| `custodianmanualallocapprovals` | CustodianManualAllocationApproval | Manual custodian allocation approval |
| `primarysubscriberallocapprovals` | PrimarySubscriberAllocationApproval | PS allocation approval |
| `primarysubscribermanualallocapprovals` | PrimarySubscriberManualAllocationApproval | Manual PS allocation approval (snapshot: `manualAllocations[].bankAccountDetails`). **TD change**: added `allocationRequestId` ref per allocation |
| `locapprovals` | LOCApproval | LOC approval workflow |
| `subscriptionperiodapprovals` | SubscriptionPeriodApproval | Subscription period approval |
| `maturityapprovals` | MaturityApproval | Maturity event approval |
| `isinstockcodeapprovals` | IsinStockCodeApproval | ISIN/Stock code approval |

## System & Integrations (9)

| Collection | Model | Purpose |
|---|---|---|
| `systemsettings` | SystemSettings | Platform config (maintenance, limits, timeouts) |
| `auditlogs` | AuditLog | Action audit trail across all modules |
| `announcements` | Announcement | Bond announcements with L1/L2 approval |
| `aiserviceconfigs` | AIServiceConfig | AI model/endpoint config |
| `files` | File | Document uploads (PocketBase refs) |
| `jobs` | Job | Background job tracking (bulk operations) |
| `maybankusers` | MaybankUser | Maybank integration user data |
| `sftpfiles` | SftpFile | SFTP uploaded files for bulk CSV imports |
| `whitelistentries` / `ipwhitelists` | WhitelistEntry / IPWhitelist | Wallet & IP whitelisting |

---

## Key Patterns

- **L1/L2 approval workflow** is repeated across ~15 collections — nearly every action needs two-level sign-off
- **5 collections store bank data snapshots** (AllocationRequest, SubAgreementRequest, BondTokenRequest, CustodianAllocationApproval, PrimarySubscriberManualAllocationApproval)
- **Snapshot vs reference** — request/approval collections copy data at submission time for audit immutability rather than storing references
- **Decimal128** is used for all monetary fields (issueAmount, nominalValue, couponAmount, etc.) for precision

## TD Branch Changes Summary

4 new collections + field additions to 6 existing collections:

| Change | Collection | What Changed |
|---|---|---|
| **NEW** | TdType | TD type registry (code, token address, provider) |
| **NEW** | WalletTdBalance | Per-wallet TD balance per type |
| **NEW** | WalletTdTransaction | Append-only TD ledger (credit/debit with balanceAfter) |
| **NEW** | TdWithdrawalRequest | TD withdrawal via TIdes (issuer/PS flows) |
| Modified | AllocationRequest | Added `isSettleViaTokenizedDeposits`, `tdTypeId`, `tdTypeSnapshot`, TIdes tracking fields |
| Modified | SubAgreementRequest | Same TD + TIdes fields as AllocationRequest |
| Modified | BondTokenRequest | Added `isSettleViaTokenizedDeposits`, `custodianSettlementType` (full_td/mixed/cash_only), `tdTypeId`, `tdTypeSnapshot` |
| Modified | User | `bankName` and `accountNumber` changed to `required: false` (TD wallets may skip bank details) |
| Modified | Wallet | `bankName` and `accountNumber` changed to `required: false` |
| Modified | PrimarySubscriberManualAllocationApproval | Added `allocationRequestId` ref per manual allocation entry |
