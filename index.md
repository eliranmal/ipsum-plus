---
---


<button id="grab-btn" title="it will be alright">copy</button>

<output id="ipsum-text"></output>
   

<script>

    const loadMap = (reversed) => {
        const map = new Map();
        let key, value;
        {% for entry in site.data.map %}
            key = reversed ? '{{ entry[1] }}' : '{{ entry[0] }}';
            value = reversed ? '{{ entry[0] }}' : '{{ entry[1] }}';
            map.set(key, value);
        {% endfor %}
        return map;
    };

    const loadTerms = (scrambled) => {
        const terms = [];
        {% for term in site.data.terms %}
            terms.push('{{ term }}');
        {% endfor %}
        if (scrambled) {
            return terms.sort(() => Math.random() - .5)
        }
        return terms;
    };

    const encodeText = (text) => {
        const map = loadMap();
        return text
            .split('')
            .map(c => map.get(c) ?? c)
            .join('');
    };

    const decodeText = (text) => {
        const reverseMap = loadMap(true);
        return text
            .split('')
            .map(c => reverseMap.get(c) ?? c)
            .join('');
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
        return loadTerms(true)
            .join(' ')
            .repeat(3);
    };

    const grabIpsum = () => {
        const dText = loadIpsum();
        navigator.clipboard.writeText(dText);
        showGrabBtnMarquee('copied to clipboard!');
    };

    const grabBtn = document.getElementById('grab-btn');
    const ipsumTextEl = document.getElementById('ipsum-text');
    const showGrabBtnMarquee = marquee(grabBtn);

    ipsumTextEl.value = encodeText(loadIpsum());

    grabBtn.addEventListener('click', grabIpsum);

</script>