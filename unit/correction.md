| ID | Description | Steps | Test Data | Pre-condition | Expected Output | Passed/Failed |
|----|-------------|-------|-----------|---------------|-----------------|---------------|
| CU-MET-01 | sentence_accuracy returns 1.0 for fully matching lists | 1. Call sentence_accuracy(preds, targets)<br>2. Assert result==1.0 | preds=["abc","def"], targets=["abc","def"] | None | 1.0 | Passed |
| CU-MET-02 | sentence_accuracy handles partial, zero, and empty inputs | 1. Call with partial pair<br>2. Call with mismatch pair<br>3. Call with empty lists | partial / mismatch / empty | None | 0.5, 0.0, 0.0 | Passed |
| CU-MET-03 | avg_levenshtein returns correct edit distances | 1. Call ("abc","abc")<br>2. Call ("abc","abd")<br>3. Call ("kitten","sitting") | three pairs | textdistance available | 0.0, 1.0, 3.0 | Passed |
| CU-MET-04 | token_level_accuracy computes max-length-denominator accuracy | 1. Call match pair<br>2. Call mismatch pair<br>3. Call partial-overlap pair | three pairs | None | 1.0, 0.0, ≈2/3 | Passed |
| CU-MET-05 | avg_jaccard_similarity returns correct set Jaccard | 1. Call match<br>2. Call disjoint<br>3. Call both-empty | three pairs | None | 1.0, 0.0, 1.0 | Passed |
| CU-MET-06 | sentence_accuracy raises ValueError on length mismatch | 1. Call with unequal-length lists inside pytest.raises | ["a"] vs ["a","b"] | None | ValueError | Passed |
| CU-MET-07 | avg_levenshtein raises ValueError on length mismatch | 1. Call with unequal lists inside pytest.raises | unequal lists | None | ValueError | Passed |
| CU-MET-08 | token_level_accuracy raises ValueError on length mismatch | 1. Call with unequal lists inside pytest.raises | unequal lists | None | ValueError | Passed |
| CU-MET-09 | avg_jaccard_similarity raises ValueError on length mismatch | 1. Call with unequal lists inside pytest.raises | unequal lists | None | ValueError | Passed |
| CU-VOC-01 | Domain vocabulary loads, lowercases, and deduplicates | 1. Write file with mixed-case lines<br>2. Call load_domain_vocab(path)<br>3. Inspect set | "Apple\nbanana\nCHERRY\n" | tmp dir | {"apple","banana","cherry"} | Passed |
| CU-VOC-02 | Missing file returns empty set without raising | 1. Call load_domain_vocab on non-existent path | non-existent path | None | set() | Passed |
| CU-VOC-03 | Blank lines and duplicates are skipped | 1. Write file with blanks/dups<br>2. Call loader | "apple\n\napple\nbanana\n\n" | tmp dir | {"apple","banana"} | Passed |
| CU-VOC-04 | UnicodeDecodeError returns empty set gracefully | 1. Write invalid UTF-8 bytes<br>2. Call loader | b"\xff\xfe\xfd\xfc" | tmp dir | set() | Passed |
| CU-DAT-01 | load_brands parses valid JSONL into BrandEntry list | 1. Write JSONL with one valid entry<br>2. Call load_brands | one valid entry | tmp dir | [BrandEntry(...)] all fields populated | Passed |
| CU-DAT-02 | load_brands returns empty list when file missing | 1. Call load_brands on non-existent path | non-existent path | None | [] | Passed |
| CU-DAT-03 | load_brands skips malformed JSON and KeyError lines | 1. Write file with {}, garbage, valid line<br>2. Call loader | three lines | tmp dir | [BrandEntry("Samsung")] only | Passed |
| CU-DAT-04 | load_deny_list skips comments and blank lines, lowercases | 1. Write file with comments, blanks, names<br>2. Call loader | "# header\nApple\nBanana\n\n# trailing\n" | tmp dir | {"apple","banana"} | Passed |
| CU-BAS-01 | _make_result sets changed=False when only case differs | 1. Call _make_result("Hello","hello",12.345) on subclass | as listed | concrete subclass | changed=False; original/corrected preserved | Passed |
| CU-BAS-02 | _make_result rounds latency to 2 decimal places | 1. Call _make_result with latency_ms=12.34567 | as listed | concrete subclass | latency_ms==12.35; changed=True | Passed |
| CU-BAS-03 | Direct instantiation of BaseCorrector raises TypeError | 1. Call BaseCorrector() inside pytest.raises | None | None | TypeError | Passed |
| CU-BY5-01 | ByT5Corrector.correct returns original on empty query without loading model | 1. Call correct("")<br>2. Inspect is_loaded | "" | stubbed transformers | corrected_query=""; changed=False; latency_ms=0.0; not loaded | Passed |
| CU-BY5-02 | Whitespace-only query passes through without loading model | 1. Call correct("   ")<br>2. Inspect is_loaded | "   " | stubbed transformers | original_query="   "; model not loaded | Passed |
| CU-BY5-03 | "correct: " prefix is added to tokenizer input | 1. Inject capturing fake tokenizer<br>2. Call correct("iphne")<br>3. Inspect captured input | "iphne" | mocked tokenizer+model | tokenizer received "correct: iphne" | Passed |
| CU-BY5-04 | Inference exception is caught; original query is returned | 1. Inject _BoomTokenizer that raises<br>2. Call correct("iphone") | "iphone" | _BoomTokenizer injected | corrected_query="iphone"; changed=False | Passed |
| CU-T5L-01 | Default num_beams is 1 | 1. Construct T5LargeCorrector<br>2. Inspect num_beams | None | None | 1 | Passed |
| CU-T5L-02 | Empty query passes through without loading model | 1. Call correct("") | "" | stubbed | corrected_query=""; not loaded | Passed |
| CU-T5L-03 | _load_lora_adapter is called when adapter_config.json is present | 1. Create dir with empty adapter_config.json<br>2. Call load() | dir+adapter file | tmp dir | _load_lora_adapter called; _has_lora=True | Passed |
| CU-T5L-04 | LoRA loading is skipped when adapter_config.json absent | 1. Create empty model dir<br>2. Call load() | dir without adapter | tmp dir | _load_lora_adapter not called; _has_lora=False | Passed |
| CU-T5L-05 | quantize=True passes load_in_8bit=True to from_pretrained | 1. Spy on T5ForConditionalGeneration.from_pretrained<br>2. Call load(quantize=True)<br>3. Inspect kwargs | model dir | tmp dir | load_in_8bit=True in kwargs | Passed |
| CU-QWN-01 | _extract_numbers returns numeric substrings | 1. Call with "iphone 15 pro 256gb"<br>2. Call with ""<br>3. Call with "3.14 apples" | three inputs | None | ["15","256"], [], ["3.14"] | Passed |
| CU-QWN-02 | _normalize collapses whitespace and trims | 1. Call with "  hello   world  "<br>2. Call with "" | two inputs | None | "hello world", "" | Passed |
| CU-QWN-03 | Guardrail accepts a clean correction | 1. Call _guardrail("iphne 15","iphone 15") | as listed | None | "iphone 15" | Passed |
| CU-QWN-04 | Guardrail rejects token-explosion corrections (ratio > 1.4) | 1. Call _guardrail with explosion output | "iphone" → "iphone 15 pro max ultra deluxe edition" | None | "iphone" (original) | Passed |
| CU-QWN-05 | Guardrail rejects token-collapse corrections (ratio < 0.6) | 1. Call _guardrail with collapse output | "iphone 15 pro 256 gb edition" → "iphone" | None | original preserved | Passed |
| CU-QWN-06 | Guardrail rejects when numbers change | 1. Call _guardrail("iphone 15 pro","iphone 16 pro") | as listed | None | "iphone 15 pro" | Passed |
| CU-QWN-07 | Guardrail returns original when output is empty/whitespace | 1. Call ("iphone","")<br>2. Call ("iphone","   ") | two pairs | None | "iphone" for both | Passed |
| CU-QWN-08 | _build_prompt uses apply_chat_template when available | 1. Inject tokenizer with apply_chat_template<br>2. Call _build_prompt("iphne") | "iphne" | mock tokenizer | wraps query in chat template; add_generation_prompt=True | Passed |
| CU-QWN-09 | _build_prompt falls back to plain text when chat template absent | 1. Inject tokenizer without method<br>2. Call _build_prompt("iphne") | "iphne" | mock tokenizer (no method) | output contains "iphne" and "Corrected:" | Passed |
| CU-COR-01 | Default model is BYT5-Large-V3 | 1. Call TypoCorrector().get_default_model() | None | None | "BYT5-Large-V3" | Passed |
| CU-COR-02 | Empty/whitespace queries pass through without model load | 1. Call correct_query("")<br>2. Call correct_query("   ") | as listed | stubbed transformers | identity return; default model not in _models | Passed |
| CU-COR-03 | list_models returns all 7 registry entries with correct keys | 1. Call list_models() | None | None | each entry has full schema; exactly one default=True (BYT5-Large-V3) | Passed |
| CU-COR-04 | correct_batch computes batch statistics correctly | 1. Inject fake model<br>2. Call correct_batch(["a","b","c"])<br>3. Inspect stats | 3 queries | fake model | total_queries=3; latency stats present; 3 results | Passed |
| CU-COR-05 | correct_batch([]) returns zero-stat batch | 1. Call correct_batch([]) | [] | None | total_queries=0; avg_latency_ms=0; results=[] | Passed |
| CU-COR-06 | Unknown model name falls back to default | 1. Call correct("hello", model="totally-unknown-model") | as listed | fake BYT5-Large-V3 | model_used=="BYT5-Large-V3" | Passed |
| CU-API-01 | GET / returns service descriptor | 1. Call endpoint via TestClient | None | TestClient with fake corrector | 200; body {service:"correction", status:"running", port:5001} | Passed |
| CU-API-02 | GET /health returns healthy | 1. Call endpoint | None | TestClient | 200; body {status:"healthy"} | Passed |
| CU-API-03 | POST /correct returns valid response on happy path | 1. POST {query:"iphne 15"} | as listed | TestClient with fake corrector | 200; corrected_query="iphone 15"; changed=True; latency_ms>=0 | Passed |
| CU-API-04 | POST /correct routes model parameter to backend | 1. POST {query:"iphne", model:"qwen-3.5-2b"} | as listed | TestClient | 200; model_used="qwen-3.5-2b" | Passed |
| CU-API-05 | Missing query field returns 422 | 1. POST {} | {} | TestClient | 422 (Pydantic validation) | Passed |
| CU-API-06 | Wrong-type query field returns 422 | 1. POST {query:["a"]} | as listed | TestClient | 422 | Passed |
| CU-API-07 | Inference exception returns 500 with detail | 1. Replace corrector with raising stub<br>2. POST {query:"x"} | as listed | TestClient | 500; detail contains "inference exploded" | Passed |
| CU-API-08 | GET /models returns descriptors and default | 1. Call endpoint | None | TestClient with fake corrector | 200; correction_models non-empty; defaults.correction="BYT5-Large-V3" | Passed |
| CU-MSK-01 | Whitespace-split tokenization preserves offsets | 1. Call tokenize("hello world") | as listed | None | [Token("hello",0,5), Token("world",6,11)] | Passed |
| CU-MSK-02 | Repeated whitespace is skipped | 1. Call tokenize("  foo   bar  ") | as listed | None | tokens at offsets 2 and 8 | Passed |
| CU-MSK-03 | Empty / whitespace-only input returns empty list | 1. Call tokenize("")<br>2. Call tokenize("   ") | empty/spaces | None | [] | Passed |
| CU-MSK-04 | Punctuation stays attached to tokens | 1. Call tokenize("iphone, samsung.") | as listed | None | ["iphone,", "samsung."] | Passed |
| CU-MSK-05 | strip_punct_edges returns unchanged for clean alpha tokens | 1. Call strip_punct_edges("iphone") | as listed | None | ("iphone", 0, 0) | Passed |
| CU-MSK-06 | strip_punct_edges removes trailing comma | 1. Call strip_punct_edges("iphone,") | as listed | None | ("iphone", 0, 1) | Passed |
| CU-MSK-07 | strip_punct_edges removes leading and trailing punctuation | 1. Call strip_punct_edges('"iphone"!') | as listed | None | ("iphone", 1, 2) | Passed |
| CU-MSK-08 | strip_punct_edges returns empty when token is all punctuation | 1. Call strip_punct_edges("...") | as listed | None | ("", ...) | Passed |
| CU-MSK-09 | make_mask produces canonical token form | 1. Call make_mask(0)<br>2. Call make_mask(42) | as listed | None | "<<M0>>", "<<M42>>" | Passed |
| CU-MSK-10 | find_masks_strict finds a single mask | 1. Search "I have a <<M0>> phone" | as listed | None | one match with index 0 | Passed |
| CU-MSK-11 | find_masks_strict finds multiple masks in order | 1. Search "<<M0>> and <<M1>> and <<M2>>" | as listed | None | indices [0,1,2] | Passed |
| CU-MSK-12 | find_masks_strict returns empty for plain text | 1. Search "plain text" | as listed | None | [] | Passed |
| CU-MSK-13 | Lenient pattern matches lowercase m | 1. Search "<<m0>> and <<m1>>" | as listed | None | indices [0, 1] | Passed |
| CU-MSK-14 | Lenient pattern matches inner whitespace | 1. Search "<< M0 >> here" | as listed | None | one match, index 0 | Passed |
| CU-MSK-15 | Lenient pattern coerces O to 0 | 1. Search "<<MO>>" | as listed | None | one match, index 0 | Passed |
| CU-MSK-16 | Lenient pattern matches single brackets | 1. Search "<M0>" | as listed | None | one match, index 0 | Passed |
| CU-MSK-17 | Shape-fallback matches corrupted mask shape | 1. Search "garbage <<X0>> something" | as listed | None | one shape match | Passed |
| CU-MSK-18 | Strict regex compiles and full-matches only canonical form | 1. fullmatch on "<<M5>>", "<<m5>>", "<<M>>" | three forms | None | match, no-match, no-match | Passed |
| CU-MSK-19 | Real-word lookup returns True/False | 1. Call is_real_word("apple")<br>2. Call is_real_word("xyz") | wordlist apple,banana,… | tmp dir | True, False | Passed |
| CU-MSK-20 | Lookup is case insensitive | 1. Call is_real_word("APPLE")<br>2. Call is_real_word("Apple") | as above | tmp dir | both True | Passed |
| CU-MSK-21 | __contains__ works | 1. Test "apple" in filter<br>2. Test "xyz" in filter | as above | tmp dir | True, False | Passed |
| CU-MSK-22 | Missing wordlist file → empty filter | 1. Init with Path("/nonexistent/file.txt") | as listed | None | len(f)==0; is_real_word(...)==False | Passed |
| CU-MSK-23 | Single brand exact match | 1. Call find_all("I have an iphone") with iPhone entry | as listed | empty deny | one match, canonical "iPhone" | Passed |
| CU-MSK-24 | Longest match wins | 1. Call find_all("samsung galaxy is good") with both entries | as listed | empty deny | one match, canonical "Samsung Galaxy" | Passed |
| CU-MSK-25 | Word boundary on left enforced | 1. Call find_all("asamsung") | as listed | Samsung entry | [] | Passed |
| CU-MSK-26 | Word boundary on right enforced | 1. Call find_all("samsungs") | as listed | Samsung entry | [] | Passed |
| CU-MSK-27 | Punctuation counts as a word boundary | 1. Call find_all("iphone, the best") | as listed | iPhone entry | one match | Passed |
| CU-MSK-28 | Deny-list blocks a brand from matching | 1. Call find_all("apple is a fruit") with deny {"apple"} | as listed | deny set | [] | Passed |
| CU-MSK-29 | Match preserves matched-text case but returns canonical | 1. Call find_all("IPHONE 15") | as listed | iPhone entry | matched_text="IPHONE"; canonical="iPhone" | Passed |
| CU-MSK-30 | Multiple distinct matches in one string | 1. Call find_all("apple vs samsung is the question") | as listed | Apple+Samsung | two matches {Apple, Samsung} | Passed |
| CU-MSK-31 | pattern_count reflects loaded patterns | 1. Inspect pattern_count() | iPhone entry | None | >= 1 | Passed |
| CU-MSK-32 | Typo caught (Levenshtein 1) | 1. Call lookup("iphne") with iPhone entry | as listed | english filter excluding iphne | match, canonical "iPhone" | Passed |
| CU-MSK-33 | English-word input is blocked | 1. Call lookup("apply") with Apple entry | english includes "apply" | filter loaded | None | Passed |
| CU-MSK-34 | Canonical-in-English-list still in fuzzy dict | 1. Call lookup("razr")<br>2. Call lookup("bsoe") | english includes "razer","bose" | Razer+Bose entries | matches Razer, Bose | Passed |
| CU-MSK-35 | Tokens shorter than min_match_length are skipped | 1. Call lookup("Lg") with min_len=2 | as listed | LG entry | None | Passed |
| CU-MSK-36 | Exact match returns None (no fuzzy needed) | 1. Call lookup("samsung") | as listed | Samsung entry | None | Passed |
| CU-MSK-37 | Per-entry min_match_length enforced | 1. Call lookup("samsng") with min_len=10 | as listed | Samsung entry | None | Passed |
| CU-MSK-38 | Deny-list blocks fuzzy lookup | 1. Call lookup("aple") with deny {"apple"} | as listed | Apple entry | None | Passed |
| CU-MSK-39 | Numeric-only tokens are skipped | 1. Call lookup("4090")<br>2. Call lookup("rtx4090") | as listed | Razer entry | None for both | Passed |
| CU-MSK-40 | dictionary_size reflects eligible entries | 1. Build with 3 eligible entries<br>2. Inspect size | iPhone+Razer+Bose | None | 3 | Passed |
| CU-MSK-41 | Exact-match brand is masked | 1. Call mask("I want an iphone") | as listed | brands fixture | <<M0>> in masked; map → "iPhone" | Passed |
| CU-MSK-42 | Typo'd brand is masked via fuzzy stage | 1. Call mask("iphne is the best") | as listed | fixture | <<M0>> present; map → "iPhone" | Passed |
| CU-MSK-43 | English word input is not masked | 1. Call mask("apply for a job") | as listed | fixture | unchanged; empty map | Passed |
| CU-MSK-44 | Multi-word brand is matched as one mask | 1. Call mask("samsung galaxy is great") | as listed | fixture | exactly one <<M; map → "Samsung Galaxy" | Passed |
| CU-MSK-45 | Plain text without brand passes through | 1. Call mask("the quick brown fox") | as listed | fixture | unchanged; empty map | Passed |
| CU-MSK-46 | Empty input handled gracefully | 1. Call mask("") | "" | fixture | ("", {}) | Passed |
| CU-MSK-47 | Brand whose canonical appears in English wordlist still works | 1. Call mask("razer blade") with "razer" in english | as listed | fixture | masked; map → "Razer" | Passed |
| CU-MSK-48 | Strict-tier unmask roundtrip | 1. Call unmask(mask("iphone is great")) | as listed | fixture | "iPhone" in result | Passed |
| CU-MSK-49 | Lenient-tier handles inner whitespace | 1. Call unmask("I love << M0 >> phones", {<<M0>>:"iPhone"}) | as listed | None | "iPhone" in result | Passed |
| CU-MSK-50 | Lenient-tier handles lowercase <<m0>> | 1. Call unmask with "<<m0>>" | as listed | None | "iPhone" in result | Passed |
| CU-MSK-51 | Lenient-tier handles <<MO>> (O for 0) | 1. Call unmask with "<<MO>>" | as listed | None | "iPhone" in result | Passed |
| CU-MSK-52 | Multi-mask strict roundtrip with two brands | 1. Call unmask("<<M0>> beats <<M1>>", {...}) | iPhone + Samsung map | None | "iPhone beats Samsung" | Passed |
| CU-MSK-53 | Empty mask map returns text unchanged | 1. Call unmask("plain text", {}) | as listed | None | "plain text" | Passed |
| CU-MSK-54 | Pipeline reports healthy when initialised cleanly | 1. Call is_healthy() | None | fixture | True | Passed |
| CU-MSK-55 | Pipeline stats() reports counts | 1. Call stats() | None | fixture | healthy=True; exact_patterns>0 | Passed |
| CU-MSK-56 | Mask-then-corrupt-then-unmask preserves brand | 1. Call mask("samsng galaxy")<br>2. Simulate model corruption<br>3. Call unmask | as listed | fixture | "Samsung" in restored output | Passed |
| CI-COR-01 | Brand-masking layer protects typo'd brand through any model | 1. Inject FakeByT5 that lowercases output<br>2. Call correct("iphone is broekn", model="byt5-large") | as listed | brand dataset loaded | corrected_query contains "iPhone" AND "broken"; changed=True; original_query preserved | Passed |
| CI-COR-02 | T5-Large-V2.1 path skips masking (it has its own pipeline) | 1. Inject FakeT5V21 asserting no <<M tokens<br>2. Call correct("iphone 15 pro", model="T5-Large-V2.1") | as listed | brand dataset loaded | model receives raw query; no mask tokens injected | Passed |
| CI-COR-03 | Plain text without any brand passes through unchanged | 1. Inject FakeModel<br>2. Call correct("the quick brown fox", model="byt5-base") | as listed | brand dataset loaded | corrected_query=="the quick brown fox"; changed=False | Passed |
| CI-COR-04 | Default BYT5-Large-V3 actually receives masked input | 1. Inject capturing FakeByT5V3<br>2. Call correct("samsung galaxy s24", model="BYT5-Large-V3") | as listed | brand dataset loaded | model received masked input containing <<M; output canonicalised to "Samsung Galaxy S24" | Passed |
| CI-API-01 | End-to-end /correct flow through orchestrator with fake model | 1. POST /correct {query:"iphne 15"} | as listed | TestClient with fake corrector | 200; corrected_query="iphone 15"; changed=True | Passed |
| CI-API-02 | /correct routes model parameter to the right backend | 1. POST /correct {query:"iphne", model:"qwen-3.5-2b"} | as listed | TestClient | 200; model_used="qwen-3.5-2b" | Passed |
| CI-API-03 | Orchestrator exception is surfaced as HTTP 500 with detail | 1. Replace corrector with stub raising RuntimeError<br>2. Call /correct | as listed | TestClient | 500; response detail contains the raised message | Passed |
| CI-API-04 | /models endpoint reflects orchestrator's list_models() output | 1. GET /models | None | TestClient | 200; data.correction_models non-empty; defaults.correction="BYT5-Large-V3" | Passed |
