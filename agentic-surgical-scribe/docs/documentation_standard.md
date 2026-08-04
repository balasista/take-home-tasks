# Operative documentation standard

*Department of Surgery, revision 7. Applies to all laparoscopic and open biliary
procedures.*

This is the rulebook the generated note is held to. It is written the way a real
institutional standard is written, which is to say it is a compliance document
and not a specification. Reading it as a specification is part of the job.

---

## 1. The note is a legal record

The operative note is a legal document and is discoverable. Three consequences
follow, and they govern everything below.

**1.1** Every statement in the note must be supported by what happened in the
operating room. A statement that is plausible, conventional, or usually true is
not thereby supported.

**1.2** An absent fact is recorded as absent. Where a required element was not
stated, the note records **"Not documented"**. It does not omit the line, and it
does not supply the customary value.

**1.3 Inference is not documentation.** A cholecystectomy that does not mention a
drain almost certainly had no drain. The note still records the drain as not
documented, because "almost certainly" is not a thing a record may assert. If the
surgeon wants it stated, the surgeon states it.

---

## 2. Mandatory elements

Every operative note carries all of the following sections. A section with no
supporting content still appears, marked "Not documented".

| Section | Requirement |
|---|---|
| Preoperative diagnosis | As stated at the surgical safety checklist. |
| Postoperative diagnosis | State explicitly, including when unchanged. |
| Procedure performed | The procedure **as completed**. See §3. |
| Critical View of Safety | See §4. Mandatory for every cholecystectomy. |
| Intraoperative cholangiogram | Performed or not performed. See §5. |
| Estimated blood loss | Numeric, in millilitres. See §6. |
| Drain | Type and site, or none, or not documented. |
| Specimen | Disposition. |
| Complications | See §7. |
| Counts | Final count status. See §8. |

---

## 3. Procedure performed

State the procedure **as completed, not as booked or consented**.

A case that begins laparoscopically and finishes through an open incision is an
open cholecystectomy. The note names the completed operation and states that
conversion occurred. Recording such a case as laparoscopic is a documentation
error and a coding error at the same time, and it is the coding error that gets
noticed, because the payer notices it.

---

## 4. Critical View of Safety

**4.1** The critical view is achieved when two and only two structures are seen
entering the gallbladder, and the lower third of the cystic plate is cleared of
fat and fibrous tissue, before any structure is clipped or divided.

**4.2** Where achieved, the note states so explicitly.

**4.3** Where it could not be achieved, the note states so and records what was
done instead. Failing to achieve the critical view is a legitimate operative
finding and bailout is good surgery. Not saying either way is the problem.

**4.4** Where the operative record does not address it, the note records **"Not
documented"** and the case is flagged to the surgeon for attention before
signature.

> This is the element the quality committee audits. A safe operation whose note
> is silent on the critical view is counted, in every audit we run, as an
> operation with no critical view. That is not a fair reading of the surgery, and
> it is the correct reading of the record.

---

## 5. Intraoperative cholangiogram

**5.1** Record as performed only where a cholangiogram was actually performed.

**5.2** The following are **not** a performed cholangiogram, and each of them
occurs in ordinary operative conversation:

- discussing whether to perform one
- declining to perform one, with or without a stated reason
- describing the circumstances in which one would be indicated
- referring to another surgeon's or another unit's routine practice

**5.3** A performed cholangiogram changes the CPT code. See §9.

---

## 6. Estimated blood loss

**6.1** Numeric, in millilitres.

**6.2** Where a figure is revised during the case, the record takes the
**surgeon's final stated figure**. Suction canister volumes reported by other
staff include irrigation and are not an estimate of blood loss.

**6.3** Where no figure is stated, record as not documented. Do not substitute a
typical value.

---

## 7. Complications

**7.1** "None" and "not addressed" are different entries and are recorded
differently.

- Record **none** where the operative record explicitly states there were no
  complications, or explicitly negates the relevant ones.
- Record **not documented** where complications were never addressed.

**7.2** The following are complications and are recorded when they occur, whether
or not they changed the conduct of the operation: gallbladder perforation with
bile spillage; spilled gallstones, whether or not retrieved; bile duct injury;
bleeding requiring intervention; visceral injury.

**7.3** A complication that was recognized and managed intraoperatively is still
a complication. Recording it is protective, not damaging.

---

## 8. Counts

**8.1** Record the **final** count status.

**8.2** A count that was initially incorrect and then reconciled is recorded as
correct. The reconciliation itself does not need to appear in the operative note,
though it appears in the nursing record.

**8.3** Where no count is recorded in the operative record, record as not
documented.

---

## 9. Coding

**9.1** Codes are selected from the department's approved tables at
`data/reference/`. A code that does not appear in those tables may not be
submitted, whatever its status in the wider code set.

**9.2** Code the procedure **as completed**:

| Situation | Code |
|---|---|
| Laparoscopic cholecystectomy | 47562 |
| Laparoscopic cholecystectomy with cholangiography | 47563 |
| Converted to open | 47600 |
| Converted to open, with cholangiography | 47605 |

**9.3** Every code carries a written rationale that names the discriminating
fact. "Laparoscopic cholecystectomy" is not a rationale. "No cholangiogram was
performed, so 47563 does not apply" is.

**9.4** Where the operative record does not establish the discriminating fact,
code the base procedure and flag the case for the surgeon. Absence of evidence
for a cholangiogram is not evidence one was performed.

---

## 10. Identifiers

**10.1** Patient name, date of birth and medical record number are stated aloud
at the surgical safety checklist. This is required by the checklist and will not
change.

**10.2** Those identifiers must not leave the department's processing boundary,
must not appear in any payload sent to a third-party service, and must not appear
in logs or traces.

**10.3** The signed note carries the case identifier only.

---

## 11. Before signature

The note is presented to the operating surgeon with the elements requiring
attention listed first: conflicting statements, elements recorded as not
documented, and any coding decision that rests on a single utterance.

A note presented with nothing flagged is a claim that the record was complete and
unambiguous. That claim is occasionally true.
