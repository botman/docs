# BotMan Documentation

The open-source chatbot development framework for cross platform chat- and voice-applications.

## Getting Started
<div class="pt-8 flex flex-wrap">
	<div class="w-1/2 lg:w-1/3 flex justify-center">
		<a class="shadow w-4/5 flex px-4 mr-4" href="/__version__/installation">
			<svg xmlns="http://www.w3.org/2000/svg" width="100" height="100" viewBox="0 0 100 100" class="w-1/3 pr-4 heroicon-laptop heroicon heroicons-lg">
		        <path class="heroicon-laptop-body heroicon-component-fill" fill-rule="nonzero" d="M100 77h-6V20a6 6 0 0 0-6-6H12a6 6 0 0 0-6 6v57H0v7l6.08 1.22c2.16.43 5.7.78 7.91.78h72.02c2.2 0 5.75-.35 7.91-.78L100 84v-7z"></path>
		        <path class="heroicon-laptop-screen-edge heroicon-component-accent heroicon-component-fill" d="M9 20a3 3 0 0 1 3-3h76a3 3 0 0 1 3 3v52a3 3 0 0 1-3 3H12a3 3 0 0 1-3-3V20z"></path>
		        <polygon class="heroicon-laptop-screen heroicon-component-fill" points="13 23 12 23 12 24 12 71 12 72 13 72 87 72 88 72 88 71 88 24 88 23 87 23"></polygon>
		        <path class="heroicon-shadows" d="M41.27 79h17.46A2 2 0 0 1 57 80H43a2 2 0 0 1-1.73-1zM5.2 83h89.6l-1.27.25c-2.04.41-5.45.75-7.52.75H13.99c-2.07 0-5.49-.34-7.52-.75L5.2 83z"></path>
		        <path class="heroicon-outline" fill-rule="nonzero" d="M100 77v7l-6.08 1.22c-2.16.43-5.7.78-7.91.78H13.99c-2.2 0-5.75-.35-7.91-.78L0 84v-7h6V20a6 6 0 0 1 6-6h76a6 6 0 0 1 6 6v57h6zm-2 5v-3H59.83A3 3 0 0 1 57 81H43a3 3 0 0 1-2.83-2H2v3h96zm-56.73-3c.34.6.99 1 1.73 1h14a2 2 0 0 0 1.73-1H41.27zM5.2 83l1.27.25c2.03.41 5.45.75 7.52.75h72.02c2.07 0 5.48-.34 7.52-.75L94.8 83H5.2zM8 77h84V20a4 4 0 0 0-4-4H12a4 4 0 0 0-4 4v57zm1-57a3 3 0 0 1 3-3h76a3 3 0 0 1 3 3v52a3 3 0 0 1-3 3H12a3 3 0 0 1-3-3V20zm3-2a2 2 0 0 0-2 2v52c0 1.1.9 2 2 2h76a2 2 0 0 0 2-2V20a2 2 0 0 0-2-2H12zm1 5h75v49H12V23h1zm74 1H13v47h74V24zm-35-4v1h-6v-1h6zm-7.55 36.22l-.9-.44 10-20 .9.44-10 20zm5-2l-.9-.44 6-12 .9.44-6 12z"></path>
		    </svg>
		    <div class="w-2/3">
		    	<h3 class="m-0">Installation</h3>
		    	<p>
					Learn how to install BotMan and get started with your first chatbot.
		    	</p>
		    </div>
		</a>
	</div>
	<div class="w-1/2 lg:w-1/3 flex justify-center">
		<a class="shadow w-4/5 flex px-4 mr-4" href="/__version__/receiving">
			<svg xmlns="http://www.w3.org/2000/svg" width="100" height="100" viewBox="0 0 100 100" class="w-1/3 pr-4 heroicon-chat heroicon heroicons-lg">
                    <path class="heroicon-chat-bubble-back heroicon-component-accent heroicon-component-fill" d="M62 12v16H47a3 3 0 1 0-6 0h-3c-1.06 0-2.08.16-3.04.47a3 3 0 1 0-3.94 2.37A9.97 9.97 0 0 0 28 38v17.17l-4 4V50H8c-3.3 0-6-2.7-6-6V12a6 6 0 0 1 6-6h48c3.3 0 6 2.7 6 6z"></path>
                    <path class="heroicon-chat-bubble-front heroicon-component-fill" d="M90 78H72v13.17L58.83 78H37.99A8 8 0 0 1 30 70V38a8 8 0 0 1 8-8h52a8 8 0 0 1 8 8v32a8 8 0 0 1-8 8z"></path>
                    <path class="heroicon-chat-dots-back heroicon-component-fill" d="M20 30a2 2 0 1 1 0-4 2 2 0 0 1 0 4zm24-4a2 2 0 0 1 2 2h-4c0-1.1.9-2 2-2zm-10 2a2 2 0 0 1-.23.93A10 10 0 0 0 32 30a2 2 0 1 1 2-2z"></path>
                    <path class="heroicon-chat-dots-front heroicon-component-accent heroicon-component-fill" d="M51 54a3 3 0 1 1-6 0 3 3 0 0 1 6 0zm16 0a3 3 0 1 1-6 0 3 3 0 0 1 6 0zm16 0a3 3 0 1 1-6 0 3 3 0 0 1 6 0z"></path>
                    <path class="heroicon-shadows" d="M30 70a9.98 9.98 0 0 0 8 4h52a9.98 9.98 0 0 0 8-4 8 8 0 0 1-8 8H72v13.17L58.83 78H37.99A8 8 0 0 1 30 70zm-2-32v17.17l-4 4V50H8c-3.3 0-6-2.7-6-6v-.71A8 8 0 0 0 8 46h16v-8a10 10 0 0 1 10-10h4a10 10 0 0 0-10 10z"></path>
                    <path class="heroicon-outline" fill-rule="nonzero" d="M28 58l-6 6V52H8c-4.41 0-8-3.59-8-8V12a8 8 0 0 1 8-8h48c4.41 0 8 3.59 8 8v16h26a10 10 0 0 1 10 10v32a10 10 0 0 1-10 10H74v16L58 80H38a10 10 0 0 1-10-10V58zm34-46c0-3.3-2.7-6-6-6H8a6 6 0 0 0-6 6v32c0 3.3 2.7 6 6 6h16v9.17l4-4V38c0-2.81 1.16-5.35 3.02-7.16a3 3 0 1 1 3.94-2.37c.96-.3 1.98-.47 3.03-.47H41a3 3 0 1 1 6 0h15V12zM44 26a2 2 0 0 0-2 2h4a2 2 0 0 0-2-2zm-10 2a2 2 0 1 0-2 2 10 10 0 0 1 1.77-1.07A2 2 0 0 0 34 28zm56 50a8 8 0 0 0 8-8V38a8 8 0 0 0-8-8H38a8 8 0 0 0-8 8v32a8 8 0 0 0 8 8h20.83L72 91.17V78h18zM6 12c0-1.1.9-2 2-2h12v1H8a1 1 0 0 0-1 1v6H6v-6zm11 16a3 3 0 1 1 6 0 3 3 0 0 1-6 0zm3 2a2 2 0 1 0 0-4 2 2 0 0 0 0 4zm28 28a4 4 0 1 1 0-8 4 4 0 0 1 0 8zm3-4a3 3 0 1 0-6 0 3 3 0 0 0 6 0zm13 4a4 4 0 1 1 0-8 4 4 0 0 1 0 8zm3-4a3 3 0 1 0-6 0 3 3 0 0 0 6 0zm17 0a4 4 0 1 1-8 0 4 4 0 0 1 8 0zm-1 0a3 3 0 1 0-6 0 3 3 0 0 0 6 0zm7 19a3 3 0 0 0 3-3V58h1v12a4 4 0 0 1-4 4H66v-1h24zM70 34v1H38v-1h32z"></path>
                </svg>
		    <div class="w-2/3">
		    	<h3 class="m-0">Hearing Messages</h3>
		    	<p>
					Let your chatbot understand incoming messages.
		    	</p>
		    </div>
		</a>
	</div>
	<div class="w-1/2 lg:w-1/3 flex justify-center">
		<a class="shadow w-4/5 flex px-4 mr-4 flex-no-wrap" href="/__version__/sending">
			<svg xmlns="http://www.w3.org/2000/svg" width="100" height="100" viewBox="0 0 100 100" class="w-1/3 pr-4 heroicon-announcement heroicon heroicons-lg">
                    <path class="heroicon-announcement-bowl heroicon-component-fill" d="M88 9.98A82.6 82.6 0 0 1 46 26.6v28.78a82.6 82.6 0 0 1 42 16.63V9.98z"></path>
                    <path class="heroicon-announcement-front heroicon-component-accent heroicon-component-fill" d="M94 36V4a2 2 0 0 0-2-2h-2v78h2a2 2 0 0 0 2-2V46h2a2 2 0 0 0 2-2v-6a2 2 0 0 0-2-2h-2z"></path>
                    <polygon class="heroicon-announcement-middle heroicon-component-accent heroicon-component-fill" points="11 57 11 25 44 25 44 57 42 57 42 53 18 53 18.4 57"></polygon>
                    <path class="heroicon-announcement-handle heroicon-component-accent heroicon-component-fill" d="M37.55 71h2.54a2 2 0 0 0 1.86-2.74l-1.77-7.43-.18.03V55H20.21l.98 9.84c.14 1.33.63 2.31 1.37 2.9a2.9 2.9 0 0 0 1.64.6l.13.01c.25.01.52 0 .8-.06l6.05 27.4A3.1 3.1 0 0 0 34 98h4.98c1.01 0 1.65-.72 1.53-1.72L37.55 71z"></path>
                    <path class="heroicon-announcement-back heroicon-component-fill" d="M3 30H1v22h2v1a3 3 0 0 0 3 3h5V26H6a3 3 0 0 0-3 3v1z"></path>
                    <path class="heroicon-shadows" d="M46 55.4v-14a82.6 82.6 0 0 1 42 16.62v14A82.6 82.6 0 0 0 46 55.4zM26.99 74.72l-1.12-5.58c.33-.1.66-.22 1-.37l9.66-4.4.71 5.7L27 74.72zM10 31v2H8a1 1 0 1 1 0-2h2zm0 6v2H8a1 1 0 1 1 0-2h2zm0 6v2H8a1 1 0 1 1 0-2h2zm0 6v2H8a1 1 0 1 1 0-2h2zm30 8v4H20v-4h-8V41h32v16h-4zm49-16h6v37a3 3 0 0 1-3 3h-3V41z"></path>
                    <path class="heroicon-outline" fill-rule="nonzero" d="M88 0h4a4 4 0 0 1 4 4v30a4 4 0 0 1 4 4v6a4 4 0 0 1-4 4v30a4 4 0 0 1-4 4h-4v-7.45A79.62 79.62 0 0 0 46 57.4V59h-4v4l1.8 4.51A4 4 0 0 1 40.1 73h-.48l2.88 23.04A3.44 3.44 0 0 1 39 100h-5a5.09 5.09 0 0 1-4.79-3.93l-5.14-25.73c-2.58-.16-4.55-2.13-4.87-5.3L18.6 59H10v-2H6a4 4 0 0 1-4-4H0V29h2a4 4 0 0 1 4-4h4v-2h36v1.6A79.62 79.62 0 0 0 88 7.46V0zm2 2v78h2a2 2 0 0 0 2-2V4a2 2 0 0 0-2-2h-2zm6 44a2 2 0 0 0 2-2v-6a2 2 0 0 0-2-2v10zM10 27H6a2 2 0 0 0-2 2v24c0 1.1.9 2 2 2h4v-3H8a2 2 0 1 1 0-4h2v-2H8a2 2 0 1 1 0-4h2v-2H8a2 2 0 1 1 0-4h2v-2H8a2 2 0 1 1 0-4h2v-3zm0 4H8a1 1 0 1 0 0 2h2v-2zM88 9.98A82.6 82.6 0 0 1 46 26.6v28.78a82.6 82.6 0 0 1 42 16.63V9.98zm-38 20.7v-1.02a84.98 84.98 0 0 0 34-12.58v1.19a85.96 85.96 0 0 1-34 12.4zM10 37H8a1 1 0 1 0 0 2h2v-2zm0 6H8a1 1 0 1 0 0 2h2v-2zm0 6H8a1 1 0 1 0 0 2h2v-2zm-8 2h1V31H2v20zm10-26v32h2V25h-2zm3 32h3.4l-.4-4h24v4h2V25H15v32zm24.38 14h.71a2 2 0 0 0 1.86-2.74l-1.77-4.43-1.61.73.8 6.44zM24.2 68.35h.13c.64.02 1.36-.13 2.12-.48l9.95-4.52L40 61.7V55H20.21l.98 9.84c.14 1.33.63 2.31 1.37 2.9a2.9 2.9 0 0 0 1.64.6zm6.98 27.33A3.1 3.1 0 0 0 34 98h4.98c1.01 0 1.65-.72 1.53-1.72l-3.99-31.9-9.66 4.4c-.34.15-.67.28-1 .37l5.3 26.53zM23 59a1 1 0 1 1 0-2 1 1 0 0 1 0 2zm15-1a1 1 0 1 1-2 0 1 1 0 0 1 2 0z"></path>
                </svg>
		    <div class="w-2/3">
		    	<h3 class="m-0">Sending Messages</h3>
		    	<p>
					Reply to your users with text, images, audio, video or even locatio data.
		    	</p>
		    </div>
		</a>
	</div>
