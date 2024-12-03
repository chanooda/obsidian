```tsx
<svg 
	fill='none'
	height='48'
	viewBox='0 0 48 48'
	width='48'
	xmlns='http://www.w3.org/2000/svg'
>
	<circle cx='4' cy='24' fill='#17C7E1' r='4'>
		<animate
			attributeName='fill-opacity'
			begin='0.1'
			dur='0.1s'
			from='#B7EEF6'
			id='1'
			repeatCount='indefinite'
			to='#17C7E1'
    >	</animate>
	</circle>
</svg>
```
`animate` 태그를 이용하여 svg에 애니메이션을 적용할 수 있다.
`attribute-name` : `animation tag`에 적용할 animation type을 설정한다.