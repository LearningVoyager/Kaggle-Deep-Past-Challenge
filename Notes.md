# Important Notes:
    - Test.csv is dummy data. The real test data is hidden on kaggle server and is only used during scoring

# Observations:
    A few things worth noticing:
        1. Numbers are interesting
            0.3333 ma-na 2 GÍN in Akkadian becomes 22 shekels in English. The translator did the math — 0.3333 minas × 60 shekels/mina = 20 shekels + 2 = 22. Your model won't do that math. It needs to learn these conversions from examples.
        
        2. Names get transliterated, not translated
            ma-nu-ba-lúm-a-šur → Mannum-balum-Aššur. The syllables get joined and normalized. This is a consistent pattern your model needs to learn.
        
        3. Logograms get expanded
            KÙ.BABBAR → silver. GÍN → shekels. ITU.KAM → month. The model needs to memorize these mappings.

        4. The structure is preserved
            Seals listed first, then the legal content, then conditions. Document structure matters.

    
## Starter/Baseline Notebook by Leiwong

    - What leiwong is doing:
        Instead of training a model to translate, he's doing something simpler: find the most similar training document to the test text, then copy-paste the relevant portion of its translation. No neural network. No fine-tuning. Just "find the closest match."

        The TF-IDF + character n-grams part is just a fancy way of measuring "how similar are two Akkadian texts?" Character n-grams work well here because Akkadian words share lots of substrings even when slightly different.

    Why it works for the dummy test data:
        The dummy test set has only ONE document, and it happens to have an 87% similarity match in training data. That's basically the same text. So copy-pasting works brilliantly for this specific case.

    Why it probably won't work as well on the real test set:
        The real test set has ~400 documents. Most of them won't have an 87% match in training. For those, you'd be copy-pasting a loosely related translation — which will score poorly on BLEU and chrF++.

    The key insight for us:
        This tells you something important — retrieval is a strong baseline and worth including. But a model that actually learns to translate Akkadian will likely beat pure retrieval on documents without close matches.

### THOUGHTS:
    TF-IDF is essentially a bag of words (or bag of characters) approach. It asks "what characters/words appear?" but completely ignores "in what order?" and "in what context?"
    
    Think of it this way:
        a-na a-lá-ḫi-im (to Ali-ahum) and a-lá-ḫi-im a-na (Ali-ahum, to) would score identically to TF-IDF — same characters, same n-grams, just reordered. But meaning can shift.

    A transformer with self-attention would understand that word order and context matter. a-na before a name means something different than a-na in the middle of a clause.

    But here's the harder truth worth noting:
        leiwong's note says Berkeley's CuneiTranslate tried T5, mT5, and NLLB — actual neural models with attention — and got BLEU < 8%. With only 1,561 training examples, neural models overfit badly. They memorize the training data instead of learning to translate.

    So the real competition question is: can you get enough good training data that a neural model actually beats retrieval?