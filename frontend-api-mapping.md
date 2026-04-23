# Frontend API-to-Collection Mapping

Maps every frontend API service to the backend collections it interacts with.

Branch: `version/CIMB-tokenized-deposits-finalised`

Base URL: `VITE_API_URL` (defaults to `http://localhost:5001`)

---

## Authentication

| Service | Endpoints | Backend Collection |
|---|---|---|
| `authAPI` | `POST /api/auth/login`, `register`, `logout`, `send-otp`, `verify-otp`, `forgot-password`, `verify-reset-otp`, `reset-password` | User |
| | `GET /api/auth/profile`, `PUT /api/auth/profile`, `PUT /api/auth/change-password` | User |

## Bonds & Securities

| Service | Endpoints | Backend Collection |
|---|---|---|
| `bondsAPI` | `GET/POST/PUT/DELETE /api/bonds/*` | Bond |
| | `POST /api/bonds/:id/submit-for-approval`, `approve-l1`, `approve-l2` | Bond |
| | `POST /api/bonds/:id/bond-creation/submit`, `approve-l1`, `approve-l2` | Bond |
| | `GET/POST/DELETE /api/bonds/:id/isin-stock-code/*` | IsinStockCodeApproval |
| | `POST /api/bonds/:id/internal/approve`, `reject` | Bond |
| | `GET/POST /api/bonds/redemption/*` | BondRedemption |
| `bondTrusteesAPI` | `GET/POST/PATCH/DELETE /api/bond-trustees/*` | BondTrustee |
| `bondFacilityAgentsAPI` | `GET/POST/PATCH/DELETE /api/bond-facility-agents/*` | BondFacilityAgent |
| `bondPrimarySubscribersAPI` | `GET/POST/PATCH/DELETE /api/bond-primary-subscribers/*` | BondPrimarySubscriber |
| `bondMarketPricesAPI` | (referenced in backend only) | BondMarketPrice |

## Users & Account Management

| Service | Endpoints | Backend Collection |
|---|---|---|
| `usersAPI` | `GET/POST/PUT/DELETE /api/users/*` | User |
| | `POST /api/users/create` | User |
| | `GET/POST/PUT /api/users/users/*` (sub-accounts) | User |
| | `GET/POST/PUT /api/users/approval-requests/*` | ApprovalRequest |
| | `GET /api/users/parent-accounts` | User |

## Role Management

| Service | Endpoints | Backend Collection |
|---|---|---|
| `investorRolesAPI` | `GET/POST/PUT/DELETE /api/investor-roles/*` | InvestorRole, ApprovalRequest |
| `custodianRolesAPI` | `GET/POST /api/custodian-roles/*` | CustodianRole, ApprovalRequest |
| `issuerRolesAPI` | `GET/POST /api/issuer-roles/*` | IssuerRole, ApprovalRequest |
| `primarySubscriberRolesAPI` | `GET/POST /api/primary-subscriber-roles/*` | PrimarySubscriberRole, ApprovalRequest |
| `facilityAgentRolesAPI` | `GET/POST /api/facility-agent-roles/*` | FacilityAgentRole, ApprovalRequest |
| `trusteeRolesAPI` | `GET/POST /api/trustee-roles/*` | TrusteeRole, ApprovalRequest |
| `superAdminRolesAPI` | `GET/POST /api/super-admin-roles/*`, `/api/roles/approval-requests/*` | SuperAdminRole, Role, ApprovalRequest |

## Wallets & Bank Accounts

| Service | Endpoints | Backend Collection |
|---|---|---|
| `walletAPI` | `GET/POST/PUT /api/wallets/*` | Wallet |
| | `GET/POST /api/wallets/approval-requests/*` | Wallet (approval sub-doc) |
| | `POST /api/wallets/:id/approval-requests/edit-bank-account` | Wallet |
| | `POST /api/wallets/:id/td-withdraw` | Wallet |

## Subscriptions & Allocations

| Service | Endpoints | Backend Collection |
|---|---|---|
| `subscriptionAPI` | `GET/POST/PUT /api/subscriptions/*` | Subscription |
| | `GET/POST /api/subscriptions/bonds/:id/period-approvals` | SubscriptionPeriodApproval |
| | `GET/POST /api/subscriptions/bonds/:id/allocation-approvals/*` | AllocationApproval |
| | `GET/POST /api/subscriptions/bonds/:id/finalize-approvals` | AllocationApproval |
| | `GET/POST /api/allocations/*` | Allocation |
| | `GET/POST /api/custodian/bonds/:id/allocation*` | CustodianAllocation |
| `allocationRequestsAPI` | `GET/POST/PUT/DELETE /api/allocation-requests/*` | AllocationRequest, LOCAmountChangeRequest |
| `locApprovalAPI` | `GET/POST/PUT /api/loc-approvals/*` | LOCApproval |

## Custodian Operations

| Service | Endpoints | Backend Collection |
|---|---|---|
| `custodianAPI` | `GET/POST/PUT/DELETE /api/custodian/sub-investors/*` | User (sub-investors) |
| | `GET/POST/PUT /api/custodian/wallets/*` | Wallet |
| | `GET/POST/PUT /api/custodian/approval-requests/*` | ApprovalRequest |
| | `GET/POST /api/custodian/bonds/:id/allocation-approvals/*` | CustodianAllocationApproval |
| | `GET/POST /api/custodian/bonds/:id/finalize-approvals` | CustodianAllocationApproval |
| | `GET/POST/PUT /api/custodian/holding-distributions/*` | CustodianHoldingDistribution, CustodianManualAllocationApproval |
| | `GET/PUT /api/custodian/payments/*` | CustodianPayment |
| | `POST /api/custodian/td-topup` | Wallet |
| | `GET /api/custodian/bond-holdings` | Holding, Allocation |
| `custodianCouponPaymentsAPI` | `GET/POST/PUT /api/custodian/coupon-payments/*` | CustodianCouponPayment |
| `custodianMaturityPaymentsAPI` | `GET/POST/PUT /api/custodian/maturity-payments/*` | CustodianMaturityPayment |

## Primary Subscriber Operations

| Service | Endpoints | Backend Collection |
|---|---|---|
| `psAPI` | `GET/POST/DELETE /api/ps/holding-distributions/*` | PsHoldingDistribution, PrimarySubscriberManualAllocationApproval |
| | `POST /api/ps/holding-distributions/bonds/:id/create-bond-token-request` | BondTokenRequest |
| `psInternalApprovalsAPI` | `GET/POST /api/ps-internal-approvals/*` | PSInternalApproval |
| `primarySubscriberAllocationAPI` | `GET /api/facility-agent/primary-subscriber/bond-token-allocations` | Holding |

## Holdings

| Service | Endpoints | Backend Collection |
|---|---|---|
| `holdingsAPI` | `GET /api/holdings/*` | Holding |
| | `GET /api/holdings/as-holder`, `as-custodian` | Holding |
| | `GET /api/holdings/bonds-with-holdings` | Holding, Bond |
| | `GET /api/holdings/trustee/investors` | Holding |

## Payments & Payouts

| Service | Endpoints | Backend Collection |
|---|---|---|
| `paymentsAPI` | `GET/POST/PUT/DELETE /api/payments/*` | Payment |
| `payoutsAPI` | `GET/POST/PUT/DELETE /api/payouts/*` | Payout |
| `redemptionsAPI` | `GET/POST/PATCH /api/redemptions/*` | Redemption |
| `redemptionPaymentsAPI` | `GET/POST/PUT/DELETE /api/redemption-payments/*` | RedemptionPayment |
| `maturityPaymentSchedulesAPI` | `GET/POST/PUT/DELETE /api/facility-agent/maturity-payment-schedules/*` | MaturityPaymentSchedule |
| `maturityPayoutsAPI` | `GET /api/maturity-payouts/*` | MaturityPayout |
| `couponDistributionRequestsAPI` | `GET/POST/PUT /api/coupon-distribution-requests/*` | CouponDistributionRequest |
| `maturityDistributionRequestsAPI` | `GET/POST/PUT /api/maturity-distribution-requests/*` | MaturityDistributionRequest |
| `fundingAPI` | `GET/POST /api/funding/*` | Payment, Bond |

## Token Management

| Service | Endpoints | Backend Collection |
|---|---|---|
| `tokenTransfersAPI` | `GET/POST/PUT /api/token-transfers/*` | TokenTransfer |
| `bondTokenRequestsAPI` | `GET /api/bond-token-requests/*` | BondTokenRequest |
| `burnRequestsAPI` | `GET/POST /api/burn-requests/*` | BurnRequest |
| `burnCashTokenRequestsAPI` | `GET/POST/DELETE /api/burn-cash-token-requests/*` | BurnCashTokenRequest |

## Workflow Approvals

| Service | Endpoints | Backend Collection |
|---|---|---|
| `maturityApprovalsAPI` | `GET/POST /api/maturity-approvals/*` | MaturityApproval |
| `locApprovalAPI` | `GET/POST/PUT /api/loc-approvals/*` | LOCApproval |

## Templates & Agreements

| Service | Endpoints | Backend Collection |
|---|---|---|
| `locTemplatesAPI` | `GET/POST/PUT/DELETE /api/loc-templates/*` | LocTemplate |
| `subAgreementTemplatesAPI` | `GET/POST/PUT/DELETE /api/sub-agreement-templates/*` | SubAgreementTemplate |
| `subAgreementRequestsAPI` | `GET/POST/PUT/DELETE /api/sub-agreement-requests/*` | SubAgreementRequest |
| `jointSubAgreementsAPI` | `GET /api/joint-sub-agreements/*` | JointSubAgreement |

## Compliance & Whitelisting

| Service | Endpoints | Backend Collection |
|---|---|---|
| `whitelistsAPI` | `GET/POST /api/whitelist/entries/*` | WhitelistEntry |
| `ipWhitelistAPI` | `GET/POST/PUT/DELETE/PATCH /api/ip-whitelist/*` | IPWhitelist |

## System & Admin

| Service | Endpoints | Backend Collection |
|---|---|---|
| `systemSettingsAPI` | `GET/PUT /api/system-settings` | SystemSettings |
| `aiServiceAPI` | `GET/PUT/POST /api/ai-service/*` | AIServiceConfig |
| `jobsAPI` | `GET /api/jobs/*` | Job |
| `filesAPI` | `GET/POST/DELETE /api/files/*` | File |
| `manualGuidesAPI` | `GET/POST/DELETE /api/manual-guides/*` | File |
| `reportsAPI` | `POST /api/reports/*` | AuditLog, Bond, Payment, User |

## Investor & Facility Agent

| Service | Endpoints | Backend Collection |
|---|---|---|
| `investorAPI` | `GET /api/investor/counterparties/*` | User, Wallet |
| `investorDashboardAPI` | `GET /api/investor/dashboard/*` | Holding, Payout, Wallet |
| `facilityAgentAPI` | `GET/POST /api/facility-agent/*` | User, Wallet |
| `announcementsAPI` | `GET/POST/PUT/DELETE /api/announcements/*` | Announcement |

---

## Tokenised Deposits (TD branch additions)

| Service | Endpoints | Backend Collection |
|---|---|---|
| `walletAPI` | `GET /api/wallets/:id/td-transactions` | WalletTdTransaction |
| | `POST /api/wallets/:id/td-withdraw` | TdWithdrawalRequest, WalletTdBalance |
| `custodianAPI` | `POST /api/custodian/td-topup` | WalletTdBalance, WalletTdTransaction |
| | `POST /api/custodian/allocation-requests/:id/confirm-td-topup` | AllocationRequest (TIdes integration) |
| `subAgreementRequestsAPI` | `POST /api/sub-agreement-requests/:id/confirm-td-topup` | SubAgreementRequest (TIdes integration) |
| (inline in CreateUser) | `GET /api/td-types` | TdType |

---

## Summary

- **46 API service objects** across 11 service files
- **250+ unique endpoints**
- Hitting **67 backend collections** (63 dev + 4 TD-specific)
- All requests use Bearer token auth (set via axios interceptor)
- L1/L2 approval pattern appears in ~15 domains
