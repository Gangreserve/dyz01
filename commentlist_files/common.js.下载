;(function (sp) {


    $.showPageErr = function () {

        $("[data-err='true']").each(function () {
            var msg = $(this).data("v");
            if (msg) {
                alert(msg);
            }
        });
    };


    $(document).ready(function () {
        $.showPageErr();
    });


    function isMobile(mobile) {

        var pattern = /^((?:13|15|18|14|17|16|19)\d{9})$/;
        return pattern.test(mobile);

    }

    var pattern = /(\w+)\[(\d+)\]/;

    /**
     * Safely encode the given string
     *
     * @param {String} str
     * @return {String}
     * @api private
     */

    var encode = function (str) {
        try {
            return encodeURIComponent(str);
        } catch (e) {
            return str;
        }
    };

    /**
     * Safely decode the string
     *
     * @param {String} str
     * @return {String}
     * @api private
     */

    var decode = function (str) {
        try {
            return decodeURIComponent(str.replace(/\+/g, ' '));
        } catch (e) {
            return str;
        }
    };


    var parse = function (str) {
        if ('string' != typeof str) return {};

        str = $.trim(str);
        if ('' == str) return {};
        if ('?' == str.charAt(0)) str = str.slice(1);

        var obj = {};
        var pairs = str.split('&');
        for (var i = 0; i < pairs.length; i ++) {
            var parts = pairs[i].split('=');
            var key = decode(parts[0]);
            var m;

            if (m = pattern.exec(key)) {
                obj[m[1]] = obj[m[1]] || [];
                obj[m[1]][m[2]] = decode(parts[1]);
                continue;
            }

            obj[parts[0]] = null == parts[1]
                ? ''
                : decode(parts[1]);
        }

        return obj;
    };

    function ajaxPost(url, reqData, callback) {
        var options = {
            url: url, method: "POST", data: reqData, dataType: "JSON", complete: function (res) {

                var data = res.responseJSON || {error: true, "reason": "系统繁忙,请稍后再试"};
                callback(null, data);

            }
        };
        if (typeof(reqData) === "object") {
            options.contentType = "application/json";
            options.processData = false;
            options.data = JSON.stringify(reqData);
        }
        $.ajax(options);

    }

    var tpl = _.template("<option value=\"<%= id %>\" <%= selected ? \"selected\" : \"\" %> ><%= text %></option>");

    function setSelect(target, dataArr, selected) {

        dataArr = _.map(dataArr, function (item) {
            var newItem = item;
            var isSelected = item.selected || (selected && selected == item.id);
            newItem.selected = isSelected;
            newItem.text = item.text || item.name;
            return tpl(newItem);
        });

        var html = dataArr.join("");
        $(target).html(html);

    }

    if (sp.fn) {
        sp.fn.showError = function (errMsg) {

            $(this).parents(".form-group").addClass("has-error").find(".error").text(errMsg);
            return this;
        };

        sp.fn.hideError = function () {

            $(this).parents(".form-group").removeClass("has-error");
            return this;
        };
    }


    sp.parseParam = parse;
    sp.isMobile = isMobile;
    sp.ajaxPost = ajaxPost;
    sp.setSelect = setSelect;

})(jQuery);