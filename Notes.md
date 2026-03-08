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