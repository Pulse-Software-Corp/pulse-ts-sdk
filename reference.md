# Reference
<details><summary><code>client.<a href="/src/Client.ts">extract</a>({ ...params }) -> Pulse.ExtractResponse</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

The primary endpoint for the Pulse API. Parses uploaded documents or remote
file URLs and returns rich markdown content with optional structured data
extraction based on user-provided schemas and extraction options.

Set `async: true` to return immediately with a job_id for polling via
GET /job/{jobId}. Otherwise processes synchronously.

To process many files at once, see [Batch Extract](api:POST/batch/extract)
or the [Batch Processing guide](/batch).
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```typescript
await client.extract({});

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**request:** `Pulse.ExtractRequest` 
    
</dd>
</dl>

<dl>
<dd>

**requestOptions:** `PulseClient.RequestOptions` 
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.<a href="/src/Client.ts">extractAsync</a>({ ...params }) -> Pulse.AsyncSubmissionResponse</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

**Deprecated**: Use `/extract` with `async: true` instead.

Starts an asynchronous extraction job. The request mirrors the
synchronous options but returns immediately with a job identifier that
clients can poll for completion status.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```typescript
await client.extractAsync({});

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**request:** `Pulse.ExtractAsyncRequest` 
    
</dd>
</dl>

<dl>
<dd>

**requestOptions:** `PulseClient.RequestOptions` 
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.<a href="/src/Client.ts">split</a>({ ...params }) -> Pulse.SplitResponse</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Identify which pages of a document contain each topic/section.
Takes an existing extraction and a list of topics, then uses AI to
identify which PDF pages contain content related to each topic.

The result is persisted with a `split_id` that can be used with
the `/schema` endpoint (split mode) for targeted schema extraction on
specific page groups.

Set `async: true` to return immediately with a job_id for polling.

To split many extractions at once, see [Batch Split](api:POST/batch/split)
or the [Batch Processing guide](/batch).
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```typescript
await client.split({
    extraction_id: "extraction_id"
});

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**request:** `Pulse.SplitInput` 
    
</dd>
</dl>

<dl>
<dd>

**requestOptions:** `PulseClient.RequestOptions` 
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.<a href="/src/Client.ts">schema</a>({ ...params }) -> Pulse.SchemaResponse</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Apply schema extraction to a previously saved extraction. The mode is
inferred from the input:

**Single mode** — Provide `extraction_id` + `schema_config` (or
`schema_config_id`) to apply one schema to the entire document.

**Multi-extraction mode** — Provide a batch extract ID as `extraction_id`
(auto-detected) or an explicit `extraction_ids` list. The content from all
extractions is combined and the schema is applied to the composite. Citations
use `extraction_id-bb_id` format to disambiguate across source documents.

**Split mode** — Provide `split_id` + `split_schema_config` to apply
different schemas to different page groups from a prior `/split` call.
Each topic can have its own schema, prompt, and effort setting.

**Excel template mode** — Provide `excel_template` (base64 .xlsx) in
`schema_config` instead of `input_schema`. The schema is auto-generated
from the template's column headers, and a filled copy is returned as
`excel_output_url`.

Creates a versioned schema record that can be retrieved later.
Set `async: true` to return immediately with a job_id for polling.

To apply schemas across many extractions or splits at once, see
[Batch Schema](api:POST/batch/schema) or the
[Batch Processing guide](/batch).
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```typescript
await client.schema();

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**request:** `Pulse.SchemaInput` 
    
</dd>
</dl>

<dl>
<dd>

**requestOptions:** `PulseClient.RequestOptions` 
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.<a href="/src/Client.ts">downloadSchemaExcel</a>({ ...params }) -> core.BinaryResponse</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Download the filled Excel template produced by a schema extraction that
used `excel_template` in its `schema_config`. Requires the same API key
authentication as other endpoints. The caller must belong to the org
that owns the underlying extraction.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```typescript
await client.downloadSchemaExcel({
    schemaId: "schemaId"
});

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**request:** `Pulse.DownloadSchemaExcelRequest` 
    
</dd>
</dl>

<dl>
<dd>

**requestOptions:** `PulseClient.RequestOptions` 
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.<a href="/src/Client.ts">tables</a>({ ...params }) -> Pulse.TablesResponse</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Extract tables from a previously completed extraction. Processes the
extraction's document content and returns structured table data.

Requires the `tables_endpoint` feature flag to be enabled for your
organization.

Set `async: true` to return immediately with a `tables_id` for
polling via `GET /job/{tables_id}`.

To extract tables from many extractions at once, see
[Batch Tables](api:POST/batch/tables) or the
[Batch Processing guide](/batch).
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```typescript
await client.tables({
    extraction_id: "extraction_id"
});

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**request:** `Pulse.TablesInput` 
    
</dd>
</dl>

<dl>
<dd>

**requestOptions:** `PulseClient.RequestOptions` 
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

## Form
<details><summary><code>client.form.<a href="/src/api/resources/form/client/Client.ts">detect</a>({ ...params }) -> Pulse.FormResult</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Run cell detection on a PDF and return the detected `form_fields`
along with a reusable `form_id`. No LLM matching, no fill, no
clear — this is the OCR / layout step that `/form/fill` and
`/form/clear` would otherwise run internally.

The returned `form_id` references the uploaded PDF and its
detected layout, and can be passed back to a subsequent
`/form/fill`, `/form/clear`, or `/form/detect` call as the
single input source — Pulse will skip detection on the fast
path and reuse the cached cells.

**Input modes** — provide exactly one of:
- `form_id` — re-detect cells on a previously stored PDF.
  Useful when callers want to refresh layout after editing or
  when chaining detect calls.
- `file_url` — public or pre-signed URL Pulse will download.
- `file` — direct binary upload of the PDF.

All three input modes ride on the same `multipart/form-data`
request body. (Callers sending `Content-Type: application/json`
with `form_id` / `file_url` are still accepted server-side for
backward compatibility, but the SDKs only model the multipart
form.)

Optional `page_range` (alias `pages`, e.g. `"1-3,5"`) restricts
the operation to a subset of pages.

Synchronous by default — returns the detected layout inline.
Set `async: true` to receive `{job_id, status: "pending"}`
immediately and poll [GET /job/{jobId}](api:GET/job/{jobId}).

Billed at **1 credit per page**. Requires the `form_filler`
feature flag to be enabled for your organization.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```typescript
await client.form.detect({});

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**request:** `Pulse.FormDetectMultipartInput` 
    
</dd>
</dl>

<dl>
<dd>

**requestOptions:** `FormClient.RequestOptions` 
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.form.<a href="/src/api/resources/form/client/Client.ts">fill</a>({ ...params }) -> Pulse.FormResult</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Fill the fields of a PDF form with values inferred from a natural
language `instructions` prompt. Works on both AcroForm PDFs
(true form fields are written) and flat/scanned PDFs (values
are rendered as an overlay using detected cells from OCR).

**Input modes** — provide exactly one of:
- `form_id` — reuse a previously processed form from a prior
  `/form/detect`, `/form/fill`, or `/form/clear` call. Skips
  re-detection (fast path); the cached `form_fields` are
  reused.
- `file_url` — public or pre-signed URL of a PDF Pulse will
  download.
- `file` — direct binary upload of the PDF. Pulse runs cell
  detection internally before filling.

All three input modes ride on the same `multipart/form-data`
request body. (Callers sending `Content-Type: application/json`
with `form_id` / `file_url` are still accepted server-side for
backward compatibility, but the SDKs only model the multipart
form.)

Optional `form_fields` lets callers supply or edit the detected
cells before filling. Optional `page_range` (alias `pages`,
e.g. `"1-3,5"`) restricts the operation to a subset of pages.

Synchronous by default — returns the filled `FormResult` inline
(including a `pdf_url` you can `GET` to download the PDF
binary). Set `async: true` to receive `{job_id, status:
"pending"}` and poll [GET /job/{jobId}](api:GET/job/{jobId}).

Billed at **3 credits per page**. Requires the `form_filler`
feature flag to be enabled for your organization.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```typescript
await client.form.fill({
    instructions: "instructions"
});

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**request:** `Pulse.FormFillMultipartInput` 
    
</dd>
</dl>

<dl>
<dd>

**requestOptions:** `FormClient.RequestOptions` 
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.form.<a href="/src/api/resources/form/client/Client.ts">clear</a>({ ...params }) -> Pulse.FormResult</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Remove user-entered data from a PDF form, leaving the blank
form template intact. Erases handwritten entries, typed values,
and unchecks selected checkboxes — printed labels, field
titles, section headers, and other static template content are
preserved.

**Input modes** — provide exactly one of:
- `form_id` — reuse a previously processed form from a prior
  `/form/detect`, `/form/fill`, or `/form/clear` call (fast
  path; cached layout reused).
- `file_url` — public or pre-signed URL of a PDF Pulse will
  download.
- `file` — direct binary upload of the PDF.

All three input modes ride on the same `multipart/form-data`
request body. (Callers sending `Content-Type: application/json`
with `form_id` / `file_url` are still accepted server-side for
backward compatibility, but the SDKs only model the multipart
form.)

`instructions` is optional. When omitted, Pulse clears every
user-filled field deterministically (no LLM call) on AcroForm
PDFs, eliminating any chance of hallucinated content. Provide
a natural language prompt to clear only specific fields
(e.g. `"clear only the address fields"`); targeted clears go
through the LLM matcher with a delete-only filter.

Optional `form_fields` and `page_range` (alias `pages`) behave
the same as on [Form Fill](api:POST/form/fill).

Synchronous by default — returns the cleared `FormResult`
inline (including a `pdf_url` you can `GET` to download the
PDF binary). Set `async: true` to receive `{job_id, status:
"pending"}` and poll [GET /job/{jobId}](api:GET/job/{jobId}).

Billed at **3 credits per page**. Requires the `form_filler`
feature flag to be enabled for your organization.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```typescript
await client.form.clear({});

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**request:** `Pulse.FormClearMultipartInput` 
    
</dd>
</dl>

<dl>
<dd>

**requestOptions:** `FormClient.RequestOptions` 
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

## Batch
<details><summary><code>client.batch.<a href="/src/api/resources/batch/client/Client.ts">extract</a>({ ...params }) -> Pulse.BatchExtractResponse</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Process multiple files in parallel. Enumerates files from an input
source (S3 prefix, local directory, or URL list), calls
[Extract](api:POST/extract) for each file, and saves results to an
output destination.

Always asynchronous — returns a batch job ID immediately.
Poll [GET /job/{jobId}](api:GET/job/{jobId}) for real-time progress
including per-file completion status.

See the [Extract](api:POST/extract) endpoint for details on
`extract_options` and the [Batch Processing guide](/batch) for
an overview of the batch pipeline.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```typescript
await client.batch.extract({
    input: {},
    output: {}
});

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**request:** `Pulse.BatchExtractInput` 
    
</dd>
</dl>

<dl>
<dd>

**requestOptions:** `BatchClient.RequestOptions` 
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.batch.<a href="/src/api/resources/batch/client/Client.ts">schema</a>({ ...params }) -> Pulse.BatchSchemaResponse</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Apply schema extraction to multiple items in parallel.
Mode is inferred from the input:

**Single mode** — Provide `extraction_ids` or `batch_extract_id`
with `schema_config` to apply one schema to each extraction.

**Split mode** — Provide `split_ids` or `batch_split_id`
with `split_schema_config` to apply per-topic schemas to each split.

Each child call goes through the full [Schema](api:POST/schema) code
path. Poll [GET /job/{jobId}](api:GET/job/{jobId}) for real-time
progress.

See the [Schema](api:POST/schema) endpoint for details on
`schema_config` and `split_schema_config`, and the
[Batch Processing guide](/batch) for an overview of the batch
pipeline.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```typescript
await client.batch.schema({
    output: {}
});

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**request:** `Pulse.BatchSchemaInput` 
    
</dd>
</dl>

<dl>
<dd>

**requestOptions:** `BatchClient.RequestOptions` 
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.batch.<a href="/src/api/resources/batch/client/Client.ts">tables</a>({ ...params }) -> Pulse.BatchTablesResponse</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Extract tables from multiple existing extractions in parallel.
Each child call goes through the full [Tables](api:POST/tables) code
path.

Extractions are identified by either a `batch_extract_id` (from a
prior [Batch Extract](api:POST/batch/extract) run) or an explicit
list of `extraction_ids`.

Poll [GET /job/{jobId}](api:GET/job/{jobId}) for real-time progress.

See the [Tables](api:POST/tables) endpoint for details on
`tables_config` and the [Batch Processing guide](/batch) for an
overview of the batch pipeline.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```typescript
await client.batch.tables({
    output: {}
});

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**request:** `Pulse.BatchTablesInput` 
    
</dd>
</dl>

<dl>
<dd>

**requestOptions:** `BatchClient.RequestOptions` 
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.batch.<a href="/src/api/resources/batch/client/Client.ts">split</a>({ ...params }) -> Pulse.BatchSplitResponse</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Split multiple existing extractions by topics in parallel.
Each child call goes through the full [Split](api:POST/split) code
path.

Extractions are identified by either a `batch_extract_id` (from a
prior [Batch Extract](api:POST/batch/extract) run) or an explicit
list of `extraction_ids`.

Poll [GET /job/{jobId}](api:GET/job/{jobId}) for real-time progress.

See the [Split](api:POST/split) endpoint for details on
`split_config` and the [Batch Processing guide](/batch) for an
overview of the batch pipeline.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```typescript
await client.batch.split({
    output: {},
    split_config: {
        split_input: [{
                name: "name"
            }]
    }
});

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**request:** `Pulse.BatchSplitInput` 
    
</dd>
</dl>

<dl>
<dd>

**requestOptions:** `BatchClient.RequestOptions` 
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

## Pipeline
<details><summary><code>client.pipeline.<a href="/src/api/resources/pipeline/client/Client.ts">execute</a>({ ...params }) -> Pulse.PipelineExecuteResponse</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Chain multiple processing steps (extract, schema, split, tables) into a
single request with inline configurations. No saved pipeline required.

The `steps` object defines what to run and in what order. Outputs flow
forward automatically — you never need to pass extraction IDs between
steps.

**Supported step combinations:**
- `extract` — extract a single document
- `extract` → `schema` — extract then apply structured schema
- `extract` → `split` — extract then split into topics
- `extract` → `split` → `schema` — extract, split by topic, apply per-topic schemas
- `extract` → `tables` — extract then extract structured tables
- `batch_extract` → `schema` — extract multiple files, combine into one schema output

**Document input:**
- Single file: provide `fileUrl` in JSON or `file` via multipart
- Multiple files (batch_extract): provide `fileUrls` in JSON or multiple `file` fields via multipart

Set `async: true` to return immediately with a `job_id` for polling via
`GET /job/{jobId}`.

Set `autoDelete: true` for zero-retention mode — all stored artifacts
are deleted immediately after you receive the results. Requires
`save_extractions` to be disabled for your organization.

Requires the `enable_adhoc_pipeline` feature flag.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```typescript
await client.pipeline.execute({
    steps: {}
});

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**request:** `Pulse.PipelineExecuteInput` 
    
</dd>
</dl>

<dl>
<dd>

**requestOptions:** `PipelineClient.RequestOptions` 
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

## Jobs
<details><summary><code>client.jobs.<a href="/src/api/resources/jobs/client/Client.ts">getJob</a>({ ...params }) -> Pulse.JobStatusResponse</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Check the status and retrieve results of an asynchronous job
(submitted via any endpoint with `async: true`).
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```typescript
await client.jobs.getJob({
    jobId: "jobId"
});

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**request:** `Pulse.GetJobRequest` 
    
</dd>
</dl>

<dl>
<dd>

**requestOptions:** `JobsClient.RequestOptions` 
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.jobs.<a href="/src/api/resources/jobs/client/Client.ts">cancelJob</a>({ ...params }) -> Pulse.JobCancellationResponse</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Attempts to cancel an asynchronous job that is currently pending
or processing. Jobs that have already completed will remain unchanged.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```typescript
await client.jobs.cancelJob({
    jobId: "jobId"
});

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**request:** `Pulse.CancelJobRequest` 
    
</dd>
</dl>

<dl>
<dd>

**requestOptions:** `JobsClient.RequestOptions` 
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

## Results
<details><summary><code>client.results.<a href="/src/api/resources/results/client/Client.ts">getPdf</a>({ ...params }) -> core.BinaryResponse</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Download the PDF binary produced by a `/form/detect`,
`/form/fill`, or `/form/clear` job. The `pdf_url` field on a
`FormResult` points at this endpoint — you can hand it
directly to a browser, embed it in an `<iframe>`, or fetch the
bytes from a backend.

Returns `404` for non-form jobs (no PDF was produced) and for
form jobs whose PDF artifact is no longer available.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```typescript
await client.results.getPdf({
    jobId: "jobId"
});

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**request:** `Pulse.GetPdfResultsRequest` 
    
</dd>
</dl>

<dl>
<dd>

**requestOptions:** `ResultsClient.RequestOptions` 
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

## LargeResults
<details><summary><code>client.largeResults.<a href="/src/api/resources/largeResults/client/Client.ts">getLargeResult</a>({ ...params }) -> Pulse.ExtractResultCore</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Download the full result for a large extraction. When `/extract`
or `GET /job/{jobId}` returns `is_url: true`, fetch the complete
result from the URL provided.  The URL is single-use: after a
successful download the resource is deleted and subsequent
requests return 410 Gone.

For form jobs (`/form/detect`, `/form/fill`, `/form/clear`)
you don't need this endpoint at all — `GET /job/{jobId}`
already returns the full `FormResult` inline under `result`,
and the `pdf_url` field points at
[GET /results/{jobId}/pdf](api:GET/results/{jobId}/pdf) for the
binary.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```typescript
await client.largeResults.getLargeResult({
    jobId: "jobId"
});

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**request:** `Pulse.GetLargeResultLargeResultsRequest` 
    
</dd>
</dl>

<dl>
<dd>

**requestOptions:** `LargeResultsClient.RequestOptions` 
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

## Webhooks
<details><summary><code>client.webhooks.<a href="/src/api/resources/webhooks/client/Client.ts">createWebhookLink</a>() -> Pulse.CreateWebhookLinkResponse</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Generates a temporary link to the Svix webhook portal where users can manage their webhook endpoints and view message logs.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```typescript
await client.webhooks.createWebhookLink();

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**requestOptions:** `WebhooksClient.RequestOptions` 
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>
