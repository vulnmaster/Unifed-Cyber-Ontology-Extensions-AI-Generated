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

### `tool-ai-ext` (Reserved)

**Namespace URI:** `https://ontology.unifiedcyberontology.org/uco/tool-ai-ext/`
**Prefix:** `tool_ai_ext`
**File:** [`ontology/tool-ai-ext.ttl`](ontology/tool-ai-ext.ttl)

Reserved placeholder for future tool modeling extensions. No terms defined yet.

### `action-ai-ext` (Reserved)

**Namespace URI:** `https://ontology.unifiedcyberontology.org/uco/action-ai-ext/`
**Prefix:** `action_ai_ext`
**File:** [`ontology/action-ai-ext.ttl`](ontology/action-ai-ext.ttl)

Reserved placeholder for future action modeling extensions. No terms defined yet.

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
