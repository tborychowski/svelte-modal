<style>
.modal-backdrop {
	position: fixed;
	top: 0;
	left: 0;
	right: 0;
	bottom: 0;
	display: none;
	align-items: center;
	justify-content: center;
	z-index: 99;
	background-color: rgba(0,0,0,0);
	transition: background-color .3s ease-out;
}
.modal-backdrop.visible { background-color: rgba(0,0,0,.6); }

.modal {
	background: #333;
	border-radius: 3px;
	margin-top: -100px;
	opacity: 0;
	transform: scale(0.8);
	transition: opacity .3s ease-out, transform .3s ease-out;
	color: #eee;
}
.visible .modal { opacity: 1; transform: scale(1); }

.modal h1 { font-size: 18px; margin: 0 0 10px; padding: 12px 20px 10px; border-bottom: 1px solid #111; }
.modal h1:empty { display: none }
.modal-content { padding: 20px; }
</style>
<div class="modal-backdrop {opened ? 'visible' : ''}" bind:this="{backdropEl}" on:click="{onBackdropClick}">
	<div class="modal">
		<h1>{title}</h1>
		<div class="modal-content" bind:this="{contentEl}">
			<div tabindex="0" class="focus-trap focus-trap-top" on:focus="{focusLast}"></div>
			<slot></slot>
			<div tabindex="0" class="focus-trap focus-trap-bottom" on:focus="{focusFirst}"></div>
		</div>
	</div>
</div>
<script>
export let title = '', backdropEl, contentEl, triggerEl, opened = false;

function focusFirst () {
	const focusable = getFocusableElements().shift();
	if (focusable) focusable.focus();
}

function focusLast () {
	const focusable = getFocusableElements().pop();
	if (focusable) focusable.focus();
}

function getFocusableElements () {
	return Array.from(contentEl.querySelectorAll('a[href],button:not([disabled]),iframe:not([disabled]),input:not([disabled]),select:not([disabled]),textarea:not([disabled]),[contentEditable]'));
}

function onBackdropClick (e) {
	if (!e.target.closest('.modal')) close();
}

function onDocKeydown (e) {
	if (e.key === 'Escape' && opened) close();
}


export function open () {
	if (opened) return;
	triggerEl = document.activeElement;
	backdropEl.style.display = 'flex';
	setTimeout(() => {
		opened = true;
		focusFirst();
		document.addEventListener('keydown', onDocKeydown);
	}, 100);
}

export function close () {
	if (!opened) return;
	setTimeout(() => opened = false);
	setTimeout(() => {
		backdropEl.style.display = 'none';
		if (triggerEl) triggerEl.focus();
		document.removeEventListener('keydown', onDocKeydown);
	}, 300);

}

</script>
