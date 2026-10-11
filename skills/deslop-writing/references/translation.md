# Idiomatic translation

Translate meaning, intent, and register into natural target-language prose for the same reader and purpose. Explicit requests for a literal gloss, aligned wording, or source-language analysis take precedence.

## Procedure

1. Infer target language, locale, audience, and register from the request and context. Ask when a missing choice materially affects the result. Keep formal/informal address consistent.
2. Read surrounding sentences to resolve idioms, humor, modals, technical terms, and referents.
3. Rebuild sentences naturally. Adjust word order, clause order, grammatical subject, and sentence boundaries as needed. Use an equivalent idiom or a plain explanation.
4. Compare every material claim, negation, condition, exception, obligation, quantity, actor, and emphasis with the source.
5. Read the target independently for imported syntax, false friends, inconsistent terms/address, and AI filler. Apply the writing defaults.

## Traps and examples

- `Das ist kein Hexenwerk`: "It's straightforward" or "It isn't difficult" in practical instructions. "It is no witchcraft" copies the words.
- `We're not out of the woods yet`: `Wir sind noch nicht über den Berg`, or a plain statement that the problem remains unresolved. Match the register.
- `Ich verstehe nur Bahnhof`: the speaker cannot understand. `Da liegt der Hund begraben`: the underlying problem. Translate each intended sense.
- `Das passt schon` may mean reassurance, acceptance, or reluctant approval. Choose "That works", "It's fine", or another contextual equivalent with the same enthusiasm.
- German `eventuell`, `aktuell`, and `bekommen` usually mean "possibly", "current", and "receive/get". Check the actual sense.
- Preserve modal strength: `muss`/"must" expresses obligation, `sollte`/"should" recommendation, and `kann`/"can" possibility or capability. Resolve whether `may` means uncertainty or permission.
- Use the established glossary and consistent technical terms. Preserve product names and API identifiers.
- Follow target conventions for address, capitalization, punctuation, dates, and decimals while retaining values. Keep currencies, units, releases, and identities; conversion or factual localization requires a request.
- Preserve supplied humor and attitude. For an untranslatable pun, retain the point and flag a material loss when useful. Use the source's facts and experiences.

## Protected content

Preserve code blocks, commands, identifiers, paths, placeholders (`{count}`, `%s`, `{{name}}`), URLs, link destinations, and machine-readable keys/values unless their localization is requested. Translate in-scope headings, visible link labels, and alt text. Metadata translation requires explicit scope.

Translate quoted prose when it is in scope, preserving meaning and attribution. Identify a translated quotation when readers could mistake it for the original wording. Keep quotations requested in the original language unchanged. Code literals remain protected even when they contain idioms or em dashes.

For explicitly requested UI/resource translation, translate human-facing values while preserving keys, placeholders, and escaping. Keep the file valid.

Remove empty staging while preserving information and voice. Add explanations only when requested or necessary to prevent a material misunderstanding. Return the translation directly unless commentary or a bilingual layout is requested.
