# Data Protection Impact Assessment (DPIA) Template

Required under Article 35 GDPR when processing is "likely to result in a high risk to the rights and freedoms of natural persons."

**When a DPIA is mandatory** (Art. 35(3) + EDPB WP248 guidelines):
- Systematic and extensive profiling with significant effects on individuals
- Large-scale processing of special category data (Art. 9) or criminal conviction data (Art. 10)
- Systematic monitoring of a publicly accessible area at large scale (CCTV, public WiFi tracking)

**When a DPIA is strongly recommended** (EDPB criteria — 2+ triggers = DPIA recommended):
- Evaluation / scoring / profiling
- Automated decision-making with legal or significant effects
- Systematic monitoring (employee monitoring, behaviour tracking)
- Sensitive data (special categories, financial, location, biometric, health)
- Large-scale processing
- Matching or combining datasets from different sources
- Data concerning vulnerable subjects (children, employees, patients)
- Innovative technology (AI, ML, IoT, biometrics)
- Processing that prevents data subjects from exercising rights or using services

---

# DATA PROTECTION IMPACT ASSESSMENT

**Document reference**: DPIA-{{REFERENCE_NUMBER}}
**Version**: {{VERSION}}
**Date**: {{DATE}}
**Prepared by**: {{PREPARER_NAME}}, {{PREPARER_ROLE}}
**Reviewed by**: {{REVIEWER_NAME}} (DPO / Legal Counsel)
**Approved by**: {{APPROVER_NAME}}, {{APPROVER_ROLE}}
**Next review date**: {{NEXT_REVIEW_DATE}}
**Status**: Draft / Under Review / Approved / Requires Prior Consultation

---

## Part 1 — Processing Overview

### 1.1 Name and description of the processing activity

**Activity name**: {{ACTIVITY_NAME}}
**Description**: {{ACTIVITY_DESCRIPTION}}

### 1.2 Nature of the processing

*(Tick all that apply)*

- [ ] Collection
- [ ] Recording
- [ ] Organisation / structuring
- [ ] Storage
- [ ] Adaptation / alteration
- [ ] Retrieval
- [ ] Consultation
- [ ] Use
- [ ] Disclosure by transmission
- [ ] Dissemination / making available
- [ ] Alignment / combination
- [ ] Restriction
- [ ] Erasure / destruction

### 1.3 Purposes of processing

| Purpose | Legal basis (Art. 6) | Special category basis (Art. 9, if applicable) |
|---------|---------------------|----------------------------------------------|
| {{PURPOSE_1}} | {{LEGAL_BASIS_1}} | {{ART_9_BASIS_1}} |
| {{PURPOSE_2}} | {{LEGAL_BASIS_2}} | {{ART_9_BASIS_2}} |

### 1.4 Controller and processor details

| Role | Organisation | Contact |
|------|-------------|---------|
| Controller | {{CONTROLLER_NAME}} | {{CONTROLLER_CONTACT}} |
| Joint controller (if any) | {{JOINT_CONTROLLER}} | {{JOINT_CONTROLLER_CONTACT}} |
| DPO | {{DPO_NAME}} | {{DPO_EMAIL}} |
| Processors involved | {{PROCESSORS_LIST}} | (see DPA schedule) |

### 1.5 Consultation

- DPO consulted: Yes / No — Date: {{DPO_CONSULTATION_DATE}} — DPO opinion: {{DPO_OPINION}}
- Data subjects consulted: Yes / No — Method: {{CONSULTATION_METHOD}} — Summary: {{CONSULTATION_SUMMARY}}
- Processor consulted: Yes / No

---

## Part 2 — Necessity and Proportionality Assessment

### 2.1 Data minimisation

| Data element | Strictly necessary for stated purpose? | If not strictly necessary, justification |
|-------------|---------------------------------------|------------------------------------------|
| {{DATA_ELEMENT_1}} | Yes / No | {{JUSTIFICATION}} |
| {{DATA_ELEMENT_2}} | Yes / No | {{JUSTIFICATION}} |

**Assessment**: Is the data collected proportionate to the processing purpose?
{{MINIMISATION_ASSESSMENT}}

### 2.2 Retention

| Data type | Retention period | Basis for retention period | Deletion method |
|-----------|-----------------|---------------------------|-----------------|
| {{DATA_TYPE_1}} | {{RETENTION_1}} | {{BASIS_1}} | {{DELETION_METHOD_1}} |

### 2.3 Accuracy

How is accuracy ensured? {{ACCURACY_MEASURES}}

### 2.4 Data subject rights

For this processing activity, how are the following rights implemented?

| Right | Implementation | Limitations (if any) |
|-------|---------------|---------------------|
| Access (Art. 15) | {{ACCESS_IMPLEMENTATION}} | {{LIMITATIONS}} |
| Rectification (Art. 16) | {{RECT_IMPLEMENTATION}} | |
| Erasure (Art. 17) | {{ERASURE_IMPLEMENTATION}} | |
| Restriction (Art. 18) | {{RESTRICTION_IMPLEMENTATION}} | |
| Portability (Art. 20) | {{PORTABILITY_IMPLEMENTATION}} | |
| Object (Art. 21) | {{OBJECTION_IMPLEMENTATION}} | |
| Automated decision-making (Art. 22) | {{ADM_IMPLEMENTATION}} | |

### 2.5 International transfers

Are data transferred outside the EEA / UK?

| Transfer | Destination | Mechanism | Risk level |
|----------|------------|-----------|-----------|
| {{TRANSFER_1}} | {{DESTINATION_1}} | {{MECHANISM_1}} | {{RISK_1}} |

### 2.6 Privacy by design measures

What technical and organisational measures are built into the system?

- [ ] Pseudonymisation
- [ ] Anonymisation (where possible)
- [ ] Encryption at rest
- [ ] Encryption in transit
- [ ] Access controls / RBAC
- [ ] Data minimisation at collection
- [ ] Default privacy-protective settings
- [ ] Audit logging
- [ ] Automatic deletion / retention enforcement
- Other: {{OTHER_MEASURES}}

---

## Part 3 — Risk Assessment

### 3.1 Risk identification

For each identified risk, assess likelihood and severity:

**Likelihood scale**: 1 = Unlikely | 2 = Possible | 3 = Likely
**Severity scale**: 1 = Minor | 2 = Significant | 3 = Severe
**Risk score** = Likelihood × Severity

| # | Risk description | Affected data subjects | Likelihood (1-3) | Severity (1-3) | Risk score | Risk level |
|---|-----------------|----------------------|-----------------|----------------|------------|------------|
| 1 | Unauthorised access / data breach | {{DATA_SUBJECTS}} | {{L}} | {{S}} | {{SCORE}} | Low/Medium/High |
| 2 | Excessive data collection | {{DATA_SUBJECTS}} | | | | |
| 3 | Purpose limitation violation (data used beyond stated purpose) | | | | | |
| 4 | Inaccurate data leading to incorrect decisions | | | | | |
| 5 | Failure to respond to data subject rights requests | | | | | |
| 6 | Unlawful international transfer | | | | | |
| 7 | Discrimination / unfair profiling outcomes | | | | | |
| 8 | Processing by unauthorised processor / sub-processor | | | | | |
| 9 | Retention beyond stated period | | | | | |
| 10 | {{CUSTOM_RISK}} | | | | | |

### 3.2 Risk scoring legend

| Score | Risk level | Action required |
|-------|-----------|-----------------|
| 1–2 | Low | Document and monitor |
| 3–4 | Medium | Implement mitigations |
| 6–9 | High | Must mitigate before processing; consider prior consultation with supervisory authority |

---

## Part 4 — Risk Treatment

### 4.1 Mitigation measures

For each medium/high risk:

| Risk # | Risk description | Mitigation measure | Residual likelihood | Residual severity | Residual score | Owner | Target date |
|--------|-----------------|-------------------|--------------------|--------------------|----------------|-------|-------------|
| 1 | {{RISK}} | {{MITIGATION}} | {{L}} | {{S}} | {{SCORE}} | {{OWNER}} | {{DATE}} |

### 4.2 Residual risk assessment

After mitigations, are any high residual risks remaining?

- [ ] No high residual risks — proceed with processing
- [ ] High residual risks remain — **prior consultation with supervisory authority required (Art. 36)**

**Overall residual risk**: Low / Medium / High
**Processing decision**: Proceed / Proceed with conditions / Suspend pending mitigation / Require prior consultation

---

## Part 5 — Sign-off and Review

### 5.1 DPO opinion

"I have reviewed this DPIA and my opinion is as follows:"
{{DPO_OPINION_TEXT}}

DPO signature: _____________________ Date: {{DATE}}

### 5.2 Controller decision

"Having considered the risks and mitigations identified, the controller has decided to:"
- [ ] Proceed with processing as described
- [ ] Proceed with processing subject to additional mitigations listed in Part 4
- [ ] Suspend processing pending completion of mitigations
- [ ] Seek prior consultation with {{SUPERVISORY_AUTHORITY}} under Art. 36

Controller representative: {{CONTROLLER_SIGNATORY}}
Signature: _____________________ Date: {{DATE}}

### 5.3 Review schedule

This DPIA must be reviewed:
- At least every {{REVIEW_FREQUENCY}} (recommended: annually for high-risk processing)
- When the processing activity changes materially
- When a new risk becomes apparent
- Following a data breach related to this processing

---

## Appendix A — Sources and References

- EDPB Guidelines on DPIA (WP248 rev.01)
- ICO DPIA guidance (UK)
- Article 35 GDPR
- {{SUPERVISORY_AUTHORITY}} list of processing operations requiring DPIA: {{AUTHORITY_LIST_LINK}}

---

## Appendix B — Processing Activity Details (Technical Annex)

*(Attach system architecture diagrams, data flow diagrams, and technical security specifications as required)*

---

*This DPIA template is based on GDPR Article 35 and EDPB WP248 guidelines. It should be completed with input from the DPO and reviewed by qualified legal counsel for high-risk processing activities.*
