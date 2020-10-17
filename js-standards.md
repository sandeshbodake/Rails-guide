```
$(document).ready(function() {
    var s,
        validator = {

            settings: {

            },

            init: function() {
                s = this.settings;
                this.bindUIActions();
            },

            bindUIActions: function() {
                this.phoneNuberValidator()
            },

            phoneNuberValidator: function() {
                $(document).on('input', '.phone-number', function() {
                    var number = $(this).val().replace(/[^\d]/g, '')
                    if (number.length == 7) {
                        number = number.replace(/(\d{3})(\d{4})/, "$1-$2");
                    } else if (number.length == 10) {
                        number = number.replace(/(\d{3})(\d{3})(\d{4})/, "($1) $2-$3");
                    }
                    $(this).val(number)
                })
            }
        }

    validator.init();
})
```
