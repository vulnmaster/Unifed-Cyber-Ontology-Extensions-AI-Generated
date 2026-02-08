# Unified Cyber Ontology Extensions (AI-Generated)

## Overview

This repository contains AI-generated extension ontologies for the [Unified Cyber Ontology (UCO)](https://unifiedcyberontology.org/). These extensions were created for **Project VIC's Autopsy-Rust project** to improve explainability of digital forensic investigation outputs.

Extensions are organized as submodules mirroring UCO's namespace structure. CASE Investigation-level extensions are maintained separately in the [CASE Ontology Extensions (AI-Generated)](https://github.com/vulnmaster/CASE-Ontology-Extensions-AI-Generated) repository.

## Extension Submodules

### `observable-ai-ext` (Active)

**Namespace URI:** `https://ontology.unifiedcyberontology.org/uco/observable-ai-ext/`
**Prefix:** `observable_ai_ext`
**File:** [`ontology/observable-ai-ext.ttl`](ontology/observable-ai-ext.ttl)

Adds three groups of concepts to UCO Observable:

#### 1. Forensic Extraction Provenance (`ForensicExtractionFacet`)

A facet attachable to any `ObservableObject` (via `uco-core:hasFacet`) that traces the observable back to its source file, record locator, byte offset, and extraction tool/action. This enables full evidence chain documentation from extracted artifact back to the raw forensic image.

| Term | Type | Description |
|------|------|-------------|
| `ForensicExtractionFacet` | Class (Facet) | Provenance metadata for an extracted observable |
| `sourceFile` | ObjectProperty | Source file from which the observable was extracted |
| `sourceRecordLocator` | DatatypeProperty | Registry path, EVTX record #, XPath, table/row ref |
| `sourceByteOffset` | DatatypeProperty | Byte offset within source file |
| `extractionTool` | ObjectProperty | Tool/module that extracted the observable |
| `extractionAction` | ObjectProperty | Action instance that performed extraction |

#### 2. Windows Event Record Enrichment (`WindowsEventRecordFacet`)

Extends `observable:EventRecordFacet` with Windows-specific fields. The base UCO EventRecordFacet has a single `observable:account` property; Windows Security events commonly reference two distinct accounts (Subject and Target). This extension adds `subjectAccount` and `targetAccount` as subproperties of `observable:account`, plus channel, provider, computer, and logon ID.

| Term | Type | Description |
|------|------|-------------|
| `WindowsEventRecordFacet` | Class (subclass of EventRecordFacet) | Windows-specific event record metadata |
| `eventChannel` | DatatypeProperty | Log channel (Security, System, etc.) |
| `eventProvider` | DatatypeProperty | Event provider/source name |
| `eventComputer` | DatatypeProperty | Computer name from event |
| `subjectAccount` | ObjectProperty (sub of `observable:account`) | Acting account (SubjectUserSid) |
| `targetAccount` | ObjectProperty (sub of `observable:account`) | Target account (TargetUserSid) |
| `logonId` | DatatypeProperty | Logon/session ID for correlation |

#### 3. Memory Image Representation (`MemoryImage`)

Minimal representation of acquired volatile memory (RAM dumps, hibernation files, pagefiles) as a data source from which processes, connections, sessions, and other volatile artifacts can be extracted.

| Term | Type | Description |
|------|------|-------------|
| `MemoryImage` | Class (ObservableObject) | An acquired memory image |
| `MemoryImageFacet` | Class (Facet) | Characteristics of the memory acquisition |
| `acquiredTime` | DatatypeProperty | When the memory was acquired |
| `memoryFormat` | DatatypeProperty | Format (raw, AFF4, Lime, etc.) |
| `architecture` | DatatypeProperty | CPU architecture (x86, amd64, arm64) |

#### 4. Digital Account Extension Properties

Properties for `uco-observable:AccountFacet` that capture Windows-specific and multi-platform account join keys beyond the base UCO `accountIdentifier`. These enable deduplication and correlation of account objects across multiple artifacts.

| Term | Type | Description |
|------|------|-------------|
| `rid` | DatatypeProperty | Windows Relative Identifier (RID) portion of SID |
| `accountDomain` | DatatypeProperty | Domain or machine name for the account |
| `profilePath` | DatatypeProperty | File system path to user's profile directory |

#### 5. Encryption Detection (`EncryptionDetectionFacet`)

A facet for files or volumes identified as encrypted or suspected of containing encryption by the Encryption Detection ingest module.

| Term | Type | Description |
|------|------|-------------|
| `EncryptionDetectionFacet` | Class (Facet) | Encryption detection/suspicion metadata |
| `detectionMethod` | DatatypeProperty | How encryption was detected (entropy, signature, etc.) |
| `encryptionType` | DatatypeProperty | Identified or suspected algorithm/container type |
| `detectionStatus` | DatatypeProperty | Whether detected or merely suspected |
| `entropyValue` | DatatypeProperty | Computed Shannon entropy (0.0 – 8.0 bits/byte) |

#### 6. YARA Match (`YaraMatchFacet`)

A facet for files matching one or more YARA rules during ingest.

| Term | Type | Description |
|------|------|-------------|
| `YaraMatchFacet` | Class (Facet) | YARA rule match metadata |
| `yaraRuleName` | DatatypeProperty | Name of the matched YARA rule |
| `yaraRuleFile` | DatatypeProperty | Path/filename of the rule file |
| `yaraRuleNamespace` | DatatypeProperty | YARA namespace of the matched rule |

#### 7. Interesting Item (`InterestingItemFacet`)

A facet for files/artifacts flagged by configurable "interesting items" rule sets.

| Term | Type | Description |
|------|------|-------------|
| `InterestingItemFacet` | Class (Facet) | Interesting item match metadata |
| `ruleSetName` | DatatypeProperty | Rule set that flagged the item |
| `ruleName` | DatatypeProperty | Specific rule that matched |
| `matchReason` | DatatypeProperty | Human-readable match explanation |

#### 8. Program Execution (`ProgramExecutionFacet`)

A facet documenting evidence of program execution from Prefetch, UserAssist, ShimCache, BAM/DAM, Amcache, and similar Windows artifacts.

| Term | Type | Description |
|------|------|-------------|
| `ProgramExecutionFacet` | Class (Facet) | Program execution evidence |
| `programName` | DatatypeProperty | Name/path of executed program |
| `executionCount` | DatatypeProperty | Number of executions recorded |
| `lastExecutionTime` | DatatypeProperty | Most recent execution timestamp |
| `executionSource` | DatatypeProperty | Artifact source (Prefetch, UserAssist, etc.) |

#### 9. Verification Result (`VerificationResultFacet`)

A facet documenting data integrity verification results from the Data Source Integrity module.

| Term | Type | Description |
|------|------|-------------|
| `VerificationResultFacet` | Class (Facet) | Integrity verification result |
| `verificationType` | DatatypeProperty | Hash algorithm used (MD5, SHA-256, etc.) |
| `expectedValue` | DatatypeProperty | Expected hash/checksum value |
| `actualValue` | DatatypeProperty | Computed hash/checksum value |
| `verificationStatus` | DatatypeProperty | Pass/fail/skipped status |

### `tool-ai-ext` (Reserved)

**Namespace URI:** `https://ontology.unifiedcyberontology.org/uco/tool-ai-ext/`
**Prefix:** `tool_ai_ext`
**File:** [`ontology/tool-ai-ext.ttl`](ontology/tool-ai-ext.ttl)

Reserved placeholder for future tool modeling extensions. No terms defined yet.

### `action-ai-ext` (Active)

**Namespace URI:** `https://ontology.unifiedcyberontology.org/uco/action-ai-ext/`
**Prefix:** `action_ai_ext`
**File:** [`ontology/action-ai-ext.ttl`](ontology/action-ai-ext.ttl)

Adds an action summary facet and a controlled vocabulary of forensic action types for documenting module execution outcomes in CASE/UCO provenance graphs.

#### 1. Action Summary (`ActionSummaryFacet`)

A facet attached to `InvestigativeAction` nodes to capture quantitative outcomes of forensic analysis actions.

| Term | Type | Description |
|------|------|-------------|
| `ActionSummaryFacet` | Class (Facet) | Processing statistics for an analysis action |
| `filesProcessed` | DatatypeProperty | Number of files analyzed |
| `artifactsCreated` | DatatypeProperty | Number of artifacts produced |
| `errorsEncountered` | DatatypeProperty | Non-fatal errors during processing |
| `processingDurationSeconds` | DatatypeProperty | Wall-clock duration in seconds |

#### 2. Forensic Action Type Vocabulary

Named individuals representing the types of forensic analysis actions performed by Autopsy-Rust modules, used as `uco-core:name` values on `InvestigativeAction` nodes.

| Term | Description |
|------|-------------|
| `IngestJob` | Top-level data source ingest orchestration |
| `ModuleExecution` | Single ingest module execution |
| `HashComputation` | Cryptographic hash computation (MD5/SHA-1/SHA-256) |
| `MimeTypeIdentification` | File type identification via magic bytes |
| `KnownFileStatusUpdate` | Hash set lookup for known/notable status |
| `KeywordSearch` | Full-text keyword/phrase/regex search |
| `FileCarving` | File recovery from unallocated space |
| `UserTagging` | Examiner-initiated artifact tagging |
| `ReportGeneration` | Output report generation |
| `DataSourceAddition` | Data source addition to a case |
| `EncryptionDetection` | Encryption indicator analysis |
| `YaraScanning` | YARA rule-based pattern matching |
| `DataSourceIntegrityVerification` | Hash-based data source verification |
| `EmbeddedFileExtraction` | Archive/compound document extraction |
| `VirtualMachineDetection` | VM disk image detection |
| `DroneDataParsing` | Drone flight log parsing |
| `AndroidArtifactExtraction` | Android app database extraction |
| `CrossCaseCorrelation` | Central repository cross-case matching |

## Non-Assertion Policy

Autopsy-Rust does **not** assert account-to-human ownership within its CASE/UCO exports. OS accounts, service accounts, and other digital identities are exported as `uco-observable:DigitalAccount` objects with machine-derived identifiers (Windows SIDs, usernames, profile paths). The mapping of digital accounts to human individuals is left to post-processing hypothesis testing workflows.

## Design Principles

- **Prefer existing UCO terms** -- only add what is genuinely missing for deep explainability
- **Extend, don't replace** -- new facets/properties are additive; base UCO terms remain unchanged
- **Subproperty hierarchy** -- `subjectAccount` and `targetAccount` are subproperties of existing `observable:account`, maintaining backward compatibility
- **Join-key-first attribution** -- all account/activity linking is done via machine identifiers (SID, RID, username+domain, logon ID, profile path), never human identity

## Compatibility

- **Base ontology:** UCO v1.4.0
- **Import chain:** Each submodule imports the relevant UCO v1.4.0 modules
- Extensions are additive and do not modify or conflict with base UCO terms

## Origin

These extension ontologies were **AI-generated** for Project VIC's Autopsy-Rust project to increase explainability of exported CASE/UCO provenance graphs in digital forensic investigations.

## License

Apache-2.0
