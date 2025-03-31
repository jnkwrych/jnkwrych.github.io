---
layout: post
title: "GPT-4 Turbo vs. GPT-4o Mini vs. GPT-3.5 Turbo: Which LLM Works Best for Data Mapping?"
description: "I tested GPT-4-turbo, GPT-4o-mini, and GPT-3.5-turbo for structured data extraction. Learn how to balance accuracy, speed, and cost in AI-driven workflows."
---

LLMs can greatly speed up prototyping for tasks like extracting structured data from PDFs, normalizing it, and mapping it to predefined schemas. I recently tested three models from OpenAI for this purpose, evaluating accuracy, cost, and speed. Here’s a brief summary of my findings.  

### My Approach  
1. **Data Extraction:** Copy structured text directly from ERP-generated PDF offers (no OCR needed).  
2. **Schema Validation:** Use [Zod](https://zod.dev/) to ensure data integrity.  
3. **LLM-Powered Normalization & Mapping:** Map product categories, identify brands, and extract key information.  

To compare model performance, I set up a small test suite with different PDF documents, each containing various formatting styles, product descriptions, and pricing structures. This allowed me to systematically measure how well each model handled the same inputs across different scenarios.  

### Models Tested  
| Model               | OpenAI SDK Name          | Quality | Speed (vs. GPT-3.5) | Cost (vs. GPT-3.5) |  
|---------------------|------------------------|---------|---------------------|--------------------|  
| [GPT-4 Turbo](https://platform.openai.com/docs/models/gpt-4-turbo) | `gpt-4-0125-preview`  | ✅✅✅ Best | ~2x slower | ~3x more expensive |  
| [GPT-4o Mini](https://platform.openai.com/docs/models/gpt-4o)  | `gpt-4o-mini`  | ✅ Decent | ~1.5x slower | ~2x more expensive |  
| [GPT-3.5 Turbo](https://platform.openai.com/docs/models/gpt-3-5) | `gpt-3.5-turbo-0125` | ✅ Decent | Fastest (Baseline) | Cheapest |  

### Prototypical Setup in TypeScript  
A simple setup for sending structured data to OpenAI’s API and normalizing it with Zod:  

```typescript
import { OpenAI } from "openai";
import { z } from "zod";

const openai = new OpenAI({ apiKey: process.env.OPENAI_API_KEY });

const productSchema = z.object({
  name: z.string(),
  brand: z.string().optional(),
  category: z.string(),
  price: z.number(),
});

async function normalizeProductData(text: string) {
  const response = await openai.chat.completions.create({
    model: "gpt-4-0125-preview",
    messages: [{ role: "system", content: "Extract structured product data from this text." }, { role: "user", content: text }],
  });

  const rawData = JSON.parse(response.choices[0].message.content || "{}");
  return productSchema.parse(rawData);
}

// Example usage
normalizeProductData("Trek Domane SL 6, Carbon Road Bike, 3499€").then(console.log).catch(console.error);
```

For a more advanced real-world example, check out [@steven-tey’s open-source LLM starter](https://github.com/steven-tey/llm-starter), which showcases a streamlined approach to working with OpenAI in a Next.js setup.  

### Key Takeaways  
- **GPT-3.5 Turbo** was surprisingly good for schema validation but struggled with very complex mappings (e.g., identifying product categories from descriptions).  
- **GPT-4 Turbo** was significantly more accurate, with simpler prompts but at a **higher cost and slower response time** (~2x slower, ~3x costlier).  
- **GPT-4o Mini** landed between the two in both quality and cost, but in my tests, its accuracy was closer to GPT-3.5 than GPT-4 Turbo, making it a less compelling option for me.  

### My Recommendation  
For **fast and cost-efficient prototyping**, use GPT-3.5 and fall back to GPT-4 Turbo **only when necessary** (e.g., for ambiguous mappings). If cost is less of a concern, go directly with GPT-4 Turbo for the best accuracy.  
