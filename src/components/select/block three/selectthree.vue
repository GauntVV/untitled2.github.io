$('select[data-menu]').each(function() {

let select = $(this),
type = select.data('menu'),
menu = $('<div />').addClass('select-menu ' + type),
button = $('<button />'),
buttonDiv = $('<div />'),
current = $('<span />').text(select.find('option:selected').text()).appendTo(buttonDiv),
arrow = $('<em />').prependTo(button);

button.css({
'--h': select.outerHeight(),
'--w': select.outerWidth()
});

select.wrap(menu);

button.append(buttonDiv).insertAfter(select);

});

$(document).on('click', '.select-menu', function(e) {

let menu = $(this),
select = menu.children('select'),
options = select.find('option'),
active = select.find('option:selected'),
button = menu.children('button'),
buttonDiv = button.children('div'),
current = buttonDiv.children('span');

if(!menu.hasClass('change')) {

let nextOption = options.eq(active.index() == options.length - 1 ? 0 : active.index() + 1),
next = $('<span />').addClass('next').text(nextOption.text()).appendTo(buttonDiv);

options.attr('selected', false);
nextOption.attr('selected', true);

menu.addClass('change');

setTimeout(() => {

next.removeClass('next');
menu.removeClass('change');
current.remove();

}, 650);

}

});
