document.addEventListener("DOMContentLoaded",() => {
let dummy = new ToiletPaperSelect({ id: "dummy" });
});

class ToiletPaperSelect {
constructor(args) {
this.label = document.querySelector(`label[for=${args.id}]`);
this.select = document.querySelector(`select[id=${args.id}]`);
this.selectBtn = document.createElement("button");
this.options = document.createElement("div");
this.buildDropdown();
}
buildDropdown() {
if (this.select !== null) {
// create a div to contain <select>
let wrapper = document.createElement("div");
wrapper.setAttribute("class","select");
this.select.parentElement.insertBefore(wrapper,this.select);
wrapper.appendChild(this.select);

// create the button to expand and collapse the dropdown
let selectBtnAttrs = {
"class": "select__button",
"type": "button",
"id": `${this.select.id}-options`,
"aria-haspopup": "listbox",
"aria-expanded": "false"
};

for (let a in selectBtnAttrs)
this.selectBtn.setAttribute(a,selectBtnAttrs[a]);

let selectBtnText = document.createTextNode(this.select.options[0].innerHTML);
this.selectBtn.appendChild(selectBtnText);
wrapper.appendChild(this.selectBtn);

// create an options div
let optionsAttrs = {
"class": "select__options",
"aria-labelledby": selectBtnAttrs.id
};
for (let a in optionsAttrs)
this.options.setAttribute(a,optionsAttrs[a]);

// then a link for each option
for (let o of this.select.options) {
let option = document.createElement("a"),
optionText = document.createTextNode(o.innerHTML),
optionAttrs = {
"href": "#",
"class": "select__option",
"data-value": o.value,
"role": "option",
"aria-selected": "false",
"tabindex": -1
};

for (let a in optionAttrs)
option.setAttribute(a,optionAttrs[a]);

option.appendChild(optionText);
this.options.appendChild(option);
}
wrapper.appendChild(this.options);

// determine the wrapper height based on the number of options
let newWrapperHeight = 4.5 * (this.select.options.length + 1) + 1.5;
wrapper.style.setProperty("--maxHeight", `${newWrapperHeight}em`);

// sync with the pre-selected option
let preselected = this.options.querySelector(`[data-value=${this.select.value}]`);
preselected.setAttribute("aria-selected",true);
this.selectBtn.innerHTML = preselected.innerHTML;

// assign event listeners
if (this.label !== null) {
this.label.addEventListener("click",() => {
if (!this.isExpanded())
this.selectBtn.focus();
else
this.closeSelect();
});
}
this.selectBtn.addEventListener("click",() => { this.openSelect(); });
this.options.addEventListener("click",e => { this.closeSelect(e); });

document.addEventListener("click",() => {
let el = document.activeElement;
if (el.parentElement.getAttribute("aria-labelledby") !== selectBtnAttrs.id)
this.closeSelect();
});
window.addEventListener("keydown",e => {
switch (e.keyCode) {
case 9: // Tab
let tabbingFromBtn = e.shiftKey && document.activeElement === this.selectBtn,
tabbingFromLast = document.activeElement === this.options.lastChild;

if (tabbingFromBtn || tabbingFromLast)
this.closeSelect();
break;
case 27: // Esc
this.closeSelect();
break;
case 32: // Spacebar
this.closeSelect(e);
break;
case 38: // Up
this.goToOption("previous");
break;
case 40: // Down
this.goToOption("next");
break;
default:
break;
}
});
}
}
isExpanded() {
return this.selectBtn.getAttribute("aria-expanded") === "true";
}
openSelect(e) {
if (!this.isExpanded()) {
// manage states
this.selectBtn.setAttribute("aria-expanded",true);

// enable tabbing
for (let n of this.options.childNodes)
n.setAttribute("tabindex",0);

// set focus to selected option
let selected = this.options.querySelector("[aria-selected=true]");
if (selected !== null)
selected.focus();
else
this.options.childNodes[0].focus();
}
}
closeSelect(e) {
if (this.isExpanded()) {
if (e && e.target.getAttribute("aria-selected")) {
// update values of both original and custom dropdowns
this.select.value = e.target.getAttribute("data-value");
this.selectBtn.innerHTML = e.target.innerHTML;

// indicate the selected item
for (let n of this.options.childNodes)
n.setAttribute("aria-selected",false);

e.target.setAttribute("aria-selected",true);
e.preventDefault();
}
// prevent tabbing while collapsed
for (let n of this.options.childNodes)
n.setAttribute("tabindex",-1);

this.selectBtn.setAttribute("aria-expanded",false);
this.selectBtn.focus();

// scroll to the top of the control after closing
let scrollToY = 0;

if (this.label !== null)
scrollToY += this.label.offsetTop;
else
scrollToY += this.selectBtn.parentElement.offsetTop;

document.documentElement.scrollTop = scrollToY - 4;
}
}
goToOption(goTo) {
if (this.isExpanded()) {
let optionLinks = this.options.querySelectorAll("a"),
activeEl = document.activeElement,
linkFound = false;

// check for the focused option
for (let l of optionLinks) {
if (activeEl === l) {
linkFound = true;
break;
}
}
// allow movement with arrows until the top or bottom option is reached
if (linkFound) {
if (goTo === "previous" && activeEl !== optionLinks[0])
activeEl.previousSibling.focus();

else if (goTo === "next" && activeEl !== optionLinks[optionLinks.length - 1])
activeEl.nextSibling.focus();
}
}
}
}