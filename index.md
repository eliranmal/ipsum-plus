---
---


<button id="grab-btn" title="it will be alright">copy</button>

<output id="ipsum-text"></output>
   

<script>

    const loadMap = (reverse) => {
        const entries = [
        {% for entry in site.data.map %}
            ['{{ entry[0] }}', '{{ entry[1] }}'],
        {% endfor %}
        ];
        if (reverse) {
            entries.map(entry => entry.reverse());
        }
        return new Map(entries);
    };

    const loadTerms = () => {
        const terms = [
        {% for term in site.data.terms %}
            '{{ term }}',
        {% endfor %}
        ];
        return terms;
    };

    const scramble = (arr) => {
        return arr.sort(() => Math.random() - .5)
    };

    const parseText = (text, reverseMap) => {
        const map = loadMap(reverseMap);
        return text
            .split('')
            .map(c => map.get(c) ?? c)
            .join('');
    };

    const encodeText = (text) => {
        return parseText(text);
    };

    const decodeText = (text) => {
        return parseText(text, true);
    };

    const marquee = (el) => {
        let timeoutId;
        const originalText = el.textContent;
        return (message) => {
            if (typeof timeoutId !== 'undefined') {
                clearTimeout(timeoutId);
                timeoutId = undefined;
            }
            el.textContent = message;
            timeoutId = setTimeout(() => {
                el.textContent = originalText;
            }, 1000 * 2);
        };
    };

    const loadIpsum = () => {
        return scramble(loadTerms())
            .join(' ')
            .repeat(3);
    };

    const grabBtn = document.getElementById('grab-btn');
    const ipsumTextEl = document.getElementById('ipsum-text');

    const showGrabBtnMarquee = marquee(grabBtn);
    grabBtn.addEventListener('click', () => {
        const dText = loadIpsum();
        navigator.clipboard.writeText(dText);
        showGrabBtnMarquee('copied to clipboard!');
    });

    ipsumTextEl.value = encodeText(loadIpsum());

</script>