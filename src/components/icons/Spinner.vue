<template>
    <svg
        xmlns="http://www.w3.org/2000/svg"
        xmlns:xlink="http://www.w3.org/1999/xlink"
        style="margin: auto; display: block; shape-rendering: auto"
        width="150px"
        height="150px"
        viewBox="0 0 100 100"
        preserveAspectRatio="xMidYMid"
    >
        <g>
            <path
                d="M50 27A23 23 0 1 0 73 50.00000000000001"
                fill="none"
                stroke="#0a0a0a"
                stroke-width="12"
            ></path>
            <path d="M49 15L49 39L61 27L49 15" fill="#0a0a0a"></path>
            <animateTransform
                attributeName="transform"
                type="rotate"
                repeatCount="indefinite"
                dur=".6s"
                values="0 50 50;360 50 50"
                keyTimes="0;1"
            ></animateTransform>
        </g>
    </svg>
</template>
