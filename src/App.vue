<template>
    <div class="container">
        <div class="row">
            <div class="col-xs-12 col-sm-8 col-sm-offset-2 col-md-6 col-md-offset-3">
                <h1>Built-in Directives</h1>
                <p v-text="'Some text'"></p>
                <p v-html="'<strong>Some text</strong>'"></p>
            </div>
        </div>
        <hr/>
        <div class="row">
            <div class="col-xs-12 col-sm-8 col-sm-offset-2 col-md-6 col-md-offset-3">
                <h1>Custom Directives</h1>
                <p v-highlight:background.delayed="'red'">Colour this</p>
                <p v-local-highlight:background.delayed.blink="{mainColour: 'red', secondColour: 'green', delay: 500}">Blinking colours</p>
            </div>
        </div>
    </div>
</template>

<script>
    export default {
        directives: {
            'local-highlight': {
                bind(el, binding, vnode) {
                    // el.style.backgroundColor = binding.value;
                    let delay = 0;
                    if (binding.modifiers['delayed']) {
                        delay = 3000;
                    }
                    
                    if (binding.modifiers['blink']) {
                        let mainColour = binding.value.mainColour;
                        let secondColour = binding.value.secondColour;                        
                        let currentColour = mainColour;

                        setTimeout(() => {
                            console.log(`currentColour: ${currentColour}`);
                            setInterval(() => {
                                currentColour === secondColour ?  currentColour = mainColour : currentColour = secondColour;
                                
                                if (binding.arg === 'background') {
                                    el.style.backgroundColor = currentColour;
                                } else {
                                    el.style.color = currentColour;
                                }
                            },binding.value.delay);
                        }, delay);
                    } else {
                        setTimeout(() => {                           
                            if (binding.arg === 'background') {
                                el.style.backgroundColor = binding.value.mainColour;
                            } else {
                                el.style.color = binding.value.mainColour;
                            }
                        }, delay);
                    }

                }
            }
        }
    }
</script>

<style>

</style>
