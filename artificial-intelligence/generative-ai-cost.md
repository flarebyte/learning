## Key Insights

1. **API prices reflect _value_, not hardware cost.**
   GPT-4o-mini costs more _per byte of VRAM_ than GPT-4.1, because the latter’s hardware is amortized over many users and uses MoE routing.

2. **MoE (Mixture of Experts)** models are _cheaper per token than dense equivalents._
   GPT-4.1 likely only activates a subset of its ~1.8T parameters per forward pass — so effective compute is far less than a dense trillion-param model.

3. **Long context drives price tiers.**
   Claude 3.5 Sonnet (1 M context) and Gemini 2.0 Pro are priced high mainly due to long-context attention memory and memory bandwidth.

## Approx “Value Index”

Here’s a rough “quality-per-cost” ranking (1 = best deal, 5 = most overpriced per capacity):

| Rank | Model                 | Value Comment                                  |
| ---- | --------------------- | ---------------------------------------------- |
| 1    | **GPT-4.1**           | Exceptional capability per dollar (likely MoE) |
| 2    | **Mistral Large 2**   | Great open pricing balance                     |
| 3    | **GPT-3.5-turbo**     | Strong legacy model; cheap                     |
| 4    | **Claude 3.5 Haiku**  | Good speed; still premium                      |
| 5    | **Claude 3.5 Sonnet** | High cost per token vs comparable compute      |

- **Under-charged (cheap per VRAM)** → GPT-4.1, GPT-3.5-turbo, Mistral Large 2
- **Over-charged (expensive per VRAM)** → Claude 3.5 Sonnet & Haiku, GPT-4o-mini
- **Flat / fair** → Gemini 2.0 Flash, Mistral Small
