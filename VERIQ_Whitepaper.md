from reportlab.lib.pagesizes import A4
from reportlab.platypus import SimpleDocTemplate, Paragraph, Spacer, PageBreak, Table, TableStyle, KeepTogether
from reportlab.lib import colors
from reportlab.lib.styles import getSampleStyleSheet, ParagraphStyle
from reportlab.lib.enums import TA_CENTER, TA_LEFT
from reportlab.lib.units import mm
from reportlab.pdfbase.ttfonts import TTFont
from reportlab.pdfbase import pdfmetrics
from pathlib import Path

out = Path("/mnt/data/VERIQ_VRQ_Whitepaper_v1.pdf")

doc = SimpleDocTemplate(
    str(out), pagesize=A4,
    rightMargin=19*mm, leftMargin=19*mm,
    topMargin=18*mm, bottomMargin=18*mm,
    title="VERIQ (VRQ) Whitepaper",
    author="VERIQ Project",
    subject="VERIQ trust and verification network concept and token design"
)

styles = getSampleStyleSheet()
styles.add(ParagraphStyle(
    name="CoverTitle", parent=styles["Title"], fontName="Helvetica-Bold",
    fontSize=31, leading=36, textColor=colors.HexColor("#0C2340"),
    alignment=TA_CENTER, spaceAfter=8
))
styles.add(ParagraphStyle(
    name="CoverSub", parent=styles["Normal"], fontName="Helvetica",
    fontSize=13, leading=19, textColor=colors.HexColor("#1769AA"),
    alignment=TA_CENTER, spaceAfter=10
))
styles.add(ParagraphStyle(
    name="H1x", parent=styles["Heading1"], fontName="Helvetica-Bold",
    fontSize=17, leading=21, textColor=colors.HexColor("#0C2340"),
    spaceBefore=10, spaceAfter=7, keepWithNext=True
))
styles.add(ParagraphStyle(
    name="H2x", parent=styles["Heading2"], fontName="Helvetica-Bold",
    fontSize=11.5, leading=15, textColor=colors.HexColor("#1769AA"),
    spaceBefore=7, spaceAfter=4, keepWithNext=True
))
styles.add(ParagraphStyle(
    name="Bodyx", parent=styles["BodyText"], fontName="Helvetica",
    fontSize=9.3, leading=13.4, spaceAfter=6, textColor=colors.HexColor("#222B35")
))
styles.add(ParagraphStyle(
    name="Smallx", parent=styles["BodyText"], fontName="Helvetica",
    fontSize=8, leading=11, textColor=colors.HexColor("#596675"), spaceAfter=4
))
styles.add(ParagraphStyle(
    name="Calloutx", parent=styles["BodyText"], fontName="Helvetica-Bold",
    fontSize=9.2, leading=13, textColor=colors.HexColor("#0C2340"),
    backColor=colors.HexColor("#EEF6FC"), borderColor=colors.HexColor("#C7DFF2"),
    borderWidth=0.5, borderPadding=8, spaceBefore=4, spaceAfter=9
))
styles.add(ParagraphStyle(
    name="TableHeadx", parent=styles["BodyText"], fontName="Helvetica-Bold",
    fontSize=8.5, leading=10, textColor=colors.white
))
styles.add(ParagraphStyle(
    name="TableCellx", parent=styles["BodyText"], fontName="Helvetica",
    fontSize=8.4, leading=11, textColor=colors.HexColor("#222B35")
))

story = []

def P(text, style="Bodyx"):
    return Paragraph(text, styles[style])

def add_section(title, paragraphs):
    story.append(P(title, "H1x"))
    for item in paragraphs:
        if isinstance(item, tuple):
            story.append(P(item[0], item[1]))
        else:
            story.append(P(item, "Bodyx"))

story += [
    Spacer(1, 35*mm),
    P("VERIQ", "CoverTitle"),
    P("VRQ • Trust &amp; Verification Network", "CoverSub"),
    Spacer(1, 4*mm),
    P("PROJECT WHITEPAPER", "CoverSub"),
    Spacer(1, 5*mm),
    P("A proposed blockchain-enabled platform for helping people assess the credibility of online businesses, crypto projects, and trading platforms through structured verification records and transparent status information.", "Calloutx"),
    Spacer(1, 8*mm),
]
cover_data = [
    [P("<b>Token</b>", "TableCellx"), P("VERIQ (VRQ)", "TableCellx")],
    [P("<b>Network / standard</b>", "TableCellx"), P("Ethereum / ERC-20", "TableCellx")],
    [P("<b>Decimals</b>", "TableCellx"), P("18", "TableCellx")],
    [P("<b>Stated initial supply</b>", "TableCellx"), P("190,000,000 VRQ", "TableCellx")],
    [P("<b>Document version</b>", "TableCellx"), P("Version 1.0 • October 2026", "TableCellx")],
    [P("<b>Project status</b>", "TableCellx"), P("Concept and proposed roadmap; feature availability must be confirmed before publication", "TableCellx")],
]
tbl = Table(cover_data, colWidths=[48*mm, 105*mm], hAlign="CENTER")
tbl.setStyle(TableStyle([
    ("BACKGROUND", (0,0), (0,-1), colors.HexColor("#F1F5F9")),
    ("GRID", (0,0), (-1,-1), 0.4, colors.HexColor("#D5DEE8")),
    ("VALIGN", (0,0), (-1,-1), "TOP"),
    ("LEFTPADDING", (0,0), (-1,-1), 8),
    ("RIGHTPADDING", (0,0), (-1,-1), 8),
    ("TOPPADDING", (0,0), (-1,-1), 7),
    ("BOTTOMPADDING", (0,0), (-1,-1), 7),
]))
story += [tbl, Spacer(1, 10*mm),
          P("This document describes the intended project direction. Planned capabilities are not represented as live services unless separately implemented and verified.", "Smallx"),
          PageBreak()]

add_section("1. Executive Summary", [
    "VERIQ is intended to be a trust and verification platform for the digital economy. Its proposed service would help users review structured information about online businesses, cryptocurrency projects, and trading platforms before deciding whether to engage with them.",
    "The proposed platform centres on verification profiles, visible status information, review or renewal dates, and clear explanations of what was checked. The aim is not to promise that any business is risk-free; rather, it is to make the scope and status of a verification review easier to understand.",
    "VRQ is the project's stated Ethereum ERC-20 token. The current token design specifies 18 decimals and a fixed initial supply of 190,000,000 VRQ, with no additional minting function in the intended contract. Token use for platform payments, partner offers, rewards, or staking depends on separate product, legal, and technical implementation."
])
add_section("2. Problem Statement", [
    "Online users encounter websites, merchants, crypto projects, and trading services whose legitimacy can be difficult to assess. Information may be incomplete, inconsistent, outdated, or difficult to compare. A logo, social media account, or self-published claim is not by itself proof of legitimacy.",
    "VERIQ proposes a structured way to present verification findings, the date and scope of checks, status changes, and relevant supporting information. The system should make limitations visible and avoid presenting a verification badge as a guarantee of future conduct, solvency, returns, or safety."
])
add_section("3. Vision, Mission, and Principles", [
    ("Vision", "H2x"),
    "Build a recognizable trust and verification layer that helps users make more informed decisions across online commerce and digital-asset services.",
    ("Mission", "H2x"),
    "Develop transparent verification workflows and public-facing profiles that communicate what was reviewed, when it was reviewed, and what the result does—and does not—mean.",
    ("Core principles", "H2x"),
    "Transparency; evidence-based checks; clear review dates; correction and appeal pathways; privacy-aware handling of submitted information; consistent standards; and no guaranteed outcomes or investment returns."
])
add_section("4. Proposed Platform and User Journey", [
    ("4.1 Verification request", "H2x"),
    "A business or project submits an application and the information required for the relevant verification category. The platform defines the required evidence, consent, fees (if any), and review scope before a review begins.",
    ("4.2 Review and assessment", "H2x"),
    "A reviewer or documented verification workflow checks the submitted information against the category's published criteria. Depending on the category, checks may include identity or business-registration evidence, website and domain consistency, contact details, disclosure quality, and other relevant public or submitted records. The exact checks must be published and actually performed; they must not be implied if they are not performed.",
    ("4.3 Public verification profile", "H2x"),
    "If implemented, a public profile can show the entity name, category, current status, date of review, review expiry or next-review date, scope, limitations, and a reference identifier. Sensitive personal or confidential documents should not be exposed publicly.",
    ("4.4 Status and renewal", "H2x"),
    "A status should reflect the platform's defined process, not an absolute guarantee. Statuses may include Pending, Verified, Expired, Suspended, or Revoked only after the rules and operational process for each status are established. Expired or revoked records should remain understandable without misleading users into thinking a prior review is current.",
    ("4.5 Corrections and appeals", "H2x"),
    "A credible system should provide a documented channel for correcting errors, responding to disputed findings, and appealing decisions. Timelines, evidence requirements, and reviewer independence should be defined before this process is advertised as available."
])
add_section("5. Verification Categories", [
    ("Online businesses and e-commerce", "H2x"),
    "Proposed checks may cover business identity evidence, contact-channel consistency, website information, policies and disclosures, and other published criteria. A verification result cannot guarantee product quality, delivery, refunds, or future business continuity.",
    ("Cryptocurrency projects", "H2x"),
    "Proposed checks may cover project disclosures, contract address consistency, source-code publication, token information, and publicly available documentation. Contract verification or a successful checklist must not be described as an audit unless a qualified audit was actually completed.",
    ("Trading platforms and financial services", "H2x"),
    "Any review in this category must clearly identify the jurisdiction and scope. It must not imply regulatory authorization, deposit protection, financial soundness, or a guarantee against fraud unless the specific claim is independently substantiated. Legal and compliance review is required before offering or marketing such assessments."
])
add_section("6. VRQ Token Design", [
    "The stated token parameters are listed below. The deployed contract and on-chain data should be checked before this whitepaper is treated as a final technical specification."
])
token_data = [
    [P("Parameter", "TableHeadx"), P("Stated specification", "TableHeadx")],
    [P("Token name", "TableCellx"), P("VERIQ", "TableCellx")],
    [P("Symbol", "TableCellx"), P("VRQ", "TableCellx")],
    [P("Blockchain", "TableCellx"), P("Ethereum", "TableCellx")],
    [P("Standard", "TableCellx"), P("ERC-20", "TableCellx")],
    [P("Decimals", "TableCellx"), P("18", "TableCellx")],
    [P("Initial supply", "TableCellx"), P("190,000,000 VRQ", "TableCellx")],
    [P("Minting", "TableCellx"), P("Intended design has no additional mint function after deployment", "TableCellx")],
    [P("Initial distribution", "TableCellx"), P("Prior contract design allocated the initial supply to the deployer address; verify against the deployed contract", "TableCellx")],
    [P("Burning", "TableCellx"), P("Earlier design discussion included burn functionality; confirm the exact deployed contract before stating it as available", "TableCellx")],
]
tt = Table(token_data, colWidths=[43*mm, 110*mm], repeatRows=1)
tt.setStyle(TableStyle([
    ("BACKGROUND", (0,0), (-1,0), colors.HexColor("#0C2340")),
    ("GRID", (0,0), (-1,-1), 0.4, colors.HexColor("#D5DEE8")),
    ("BACKGROUND", (0,1), (0,-1), colors.HexColor("#F1F5F9")),
    ("VALIGN", (0,0), (-1,-1), "TOP"),
    ("LEFTPADDING", (0,0), (-1,-1), 7),
    ("RIGHTPADDING", (0,0), (-1,-1), 7),
    ("TOPPADDING", (0,0), (-1,-1), 6),
    ("BOTTOMPADDING", (0,0), (-1,-1), 6),
]))
story += [tt, Spacer(1, 5*mm)]
add_section("7. Proposed VRQ Utility", [
    "The following are potential utility directions, not statements that these services are already live. Each requires implementation, clear terms, operational funding, and legal review.",
    ("Platform service payments", "H2x"),
    "VRQ could be explored as a payment option for eligible verification applications or platform services. The platform would need a defined pricing method, payment flow, refund policy, and handling for network fees and price volatility.",
    ("Partner offers and discounts", "H2x"),
    "A partner programme could offer discounts or benefits to eligible users. Benefits would depend on executed partner agreements and published terms; no partner should be implied to exist before a confirmed agreement.",
    ("Rewards and membership tiers", "H2x"),
    "A future dashboard could show eligible rewards, offer history, and membership tiers such as Bronze, Silver, Gold, and Platinum. Eligibility criteria, reward funding, anti-abuse rules, and redemption conditions must be defined before launch.",
    ("Staking", "H2x"),
    "Staking has been considered as a possible future feature, including fixed-duration options. It is not a live utility merely because it appears in a roadmap. A staking system would require smart-contract design, independent security review, clearly disclosed risks, and legal review. No yield or return is guaranteed.",
    ("Important distinction", "H2x"),
    "Holding VRQ does not automatically mean a user has governance rights, ownership in a company, revenue rights, guaranteed discounts, guaranteed rewards, or a right to investment returns. Such rights must not be claimed unless formally created and legally supported."
])
add_section("8. Proposed Rewards Dashboard", [
    "A future dashboard concept discussed for VERIQ includes wallet connection, VRQ balance display, reward history, partner offers, and tier-based benefits. Possible 30-, 90-, and 180-day staking or membership options have been discussed as product concepts, not confirmed live offerings.",
    "Potential reward sources discussed include partner-funded discounts, a preallocated token pool, memberships, or referral programmes. Before launch, the project should publish the actual funding source, eligibility rules, token allocation, limits, expiration rules, and how any reward is calculated. Rewards should not be advertised as guaranteed income."
])
add_section("9. Token Distribution and Treasury Transparency", [
    "This document does not invent a token allocation percentage. The project's final public materials should disclose the actual distribution, including deployer or team holdings, any treasury, liquidity allocation, ecosystem incentives, vesting schedules, lockups, and relevant wallet addresses where appropriate.",
    "If the entire initial supply was assigned to the deployer at deployment, that fact should be clearly disclosed and not described as a community distribution. Any later transfers or allocations should be reflected accurately. A fixed supply does not by itself prevent concentration of ownership or market volatility."
])
add_section("10. Technical Architecture (Proposed)", [
    "The token layer uses the Ethereum ERC-20 standard. The verification platform itself may require application services for submissions, evidence handling, reviewer workflows, profile publishing, status updates, wallet integration, and audit logs. These platform components are separate from the token contract and must be built and tested independently.",
    "Not every verification record needs to be stored on-chain. Sensitive documents should generally remain in appropriately protected systems; a public profile may expose only the minimum information needed. If cryptographic hashes or on-chain references are used, the project should document what they prove and what they do not prove.",
    "Security practices should include controlled access, secure key management, backups, incident response, dependency review, testing, and independent smart-contract review where applicable. A source-code verification badge on a block explorer is not a security audit."
])
add_section("11. Roadmap", [
    ("Phase 1 — Token and project foundation", "H2x"),
    "Confirm deployed contract details; publish the contract address and token specification; establish official communication channels; and publish the verification criteria and project limitations.",
    ("Phase 2 — Verification workflow MVP", "H2x"),
    "Build application intake, review records, public verification profiles, status and expiry handling, and a correction or appeal process. Launch only after testing and operational procedures are in place.",
    ("Phase 3 — Wallet and partner features", "H2x"),
    "Evaluate wallet connection, VRQ payment options, partner offers, and dashboard features. Launch only those functions that have working implementations and clear terms.",
    ("Phase 4 — Rewards or staking, if approved", "H2x"),
    "Define the economic model, funding, eligibility, legal treatment, and risk disclosures. Complete independent security review before any staking contract or reward mechanism is promoted.",
    ("Roadmap status", "H2x"),
    "These phases describe proposed work. They are not promises of delivery dates and do not indicate that the listed features are currently live."
])
add_section("12. Governance, Operations, and Accountability", [
    "VERIQ should publish who operates the verification service, how reviewers are selected, how conflicts of interest are handled, how decisions can be challenged, and how status changes are recorded. If these procedures are not yet operational, they should be described as work in progress.",
    "The project should distinguish between token-contract administration and platform verification decisions. A token's owner or deployer does not automatically provide independent verification. Any future governance mechanism must be documented before it is represented as active."
])
add_section("13. Risk Factors", [
    "Technology risk: smart contracts, wallets, websites, and connected services may contain vulnerabilities or suffer outages.",
    "Verification risk: submitted information may be false, incomplete, or become outdated. A review is limited to its disclosed scope and date.",
    "Market risk: VRQ may be volatile, illiquid, or lose some or all of its market value. No exchange listing or market liquidity is guaranteed.",
    "Adoption risk: the platform may not attract enough users, reviewers, or partners to become sustainable.",
    "Regulatory risk: laws for tokens, verification services, staking, rewards, consumer protection, and financial services vary by jurisdiction and may change.",
    "Operational and privacy risk: handling business documents and user data creates confidentiality, access-control, and data-protection responsibilities.",
    "Third-party risk: blockchain networks, hosting providers, wallet providers, and partners may change or become unavailable."
])
add_section("14. Legal and Informational Disclaimer", [
    "This whitepaper is informational and describes the project's stated design and proposed direction. It is not an offer to sell securities or other financial instruments, and it is not investment, financial, legal, or tax advice. No profit, price appreciation, liquidity, platform adoption, verification outcome, reward, or staking return is promised.",
    "The legal classification and permitted distribution of VRQ and any related services depend on the facts and applicable law. The project should obtain qualified legal advice before marketing token sales, staking, rewards, or verification services in any jurisdiction.",
    "Readers should independently review the deployed smart contract, official project channels, and applicable risks before interacting with VRQ. Never rely on a verification status as a guarantee that an entity is safe, honest, solvent, or suitable for investment."
])
add_section("15. Official Project Details", [
    "<b>Project:</b> VERIQ",
    "<b>Token:</b> VERIQ (VRQ)",
    "<b>Network:</b> Ethereum",
    "<b>Contract address:</b> Add the confirmed deployed address before public release.",
    "<b>Official website / contact:</b> Add only confirmed official links.",
    "<b>Document version:</b> 1.0 • October 2026",
    ("Publication checklist", "H2x"),
    "Before publication, verify the deployed contract address, exact contract functions (including burn and ownership behaviour), token supply and deployer balance; add official project links; identify which platform features are live; and have the document reviewed for technical and legal accuracy."
])

def footer(canvas, doc):
    canvas.saveState()
    w, h = A4
    canvas.setStrokeColor(colors.HexColor("#D5DEE8"))
    canvas.line(19*mm, 14*mm, w-19*mm, 14*mm)
    canvas.setFont("Helvetica", 7.5)
    canvas.setFillColor(colors.HexColor("#596675"))
    canvas.drawString(19*mm, 9*mm, "VERIQ (VRQ) • Project Whitepaper • Version 1.0")
    canvas.drawRightString(w-19*mm, 9*mm, f"Page {doc.page}")
    canvas.restoreState()

doc.build(story, onFirstPage=footer, onLaterPages=footer)
print(f"Created whitepaper: {out} ({out.stat().st_size:,} bytes)")
