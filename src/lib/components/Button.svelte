<script lang="ts">
	import ButtonIcon from '$components/ButtonIcon/ButtonIcon.svelte';
	import Text from '$components/Text/Text.svelte';

	type Props = {
		disabled?: boolean;
		variant?: 'primary' | 'secondary' | 'persuasive';
		label: string;
		className?: string;
		alignSelf?: 'start' | 'center' | 'end';
		stretch?: boolean;
		maxWidth?: boolean;
		icon?: {
			position: 'left' | 'right';
			url: string;
		};
	};

	type LinkProps = Props & {
		as: 'link';
		href: string;
		isExternal?: boolean;
		onClick?: () => void;
	};
	type ButtonProps = Props & {
		as: 'button';
		type?: 'button' | 'submit';
		onClick: () => void;
	};

	const props: LinkProps | ButtonProps = $props();
	const {
		disabled,
		variant = 'primary',
		className,
		alignSelf,
		stretch,
		maxWidth,
		label,
		icon
	} = props;
	const classes = [
		'button',
		variant,
		className,
		`align-self-${alignSelf ?? 'start'}`,
		stretch && 'stretch',
		maxWidth && 'max-width'
	];
</script>

{#if props.as === 'link'}
	<a
		href={props.href}
		class={classes}
		aria-disabled={disabled}
		target={props.isExternal ? '_blank' : undefined}
		rel={props.isExternal ? 'noopener noreferrer' : undefined}
		onclick={props.onClick}
	>
		{#if icon && icon.position === 'left'}
			<ButtonIcon url={icon.url} position={icon.position} />
		{/if}
		<Text variant="span" styling="body-lg-medium">
			{label}
		</Text>
		{#if icon && icon.position === 'right'}
			<ButtonIcon url={icon.url} position={icon.position} />
		{/if}
	</a>
{:else if props.as === 'button'}
	<button
		onclick={() => !disabled && props.onClick()}
		class={classes}
		aria-disabled={disabled}
		type={props.type ?? 'button'}
	>
		{#if icon && icon.position === 'left'}
			<ButtonIcon url={icon.url} position={icon.position} />
		{/if}
		<Text variant="span" styling="body-lg-medium">
			{label}
		</Text>
		{#if icon && icon.position === 'right'}
			<ButtonIcon url={icon.url} position={icon.position} />
		{/if}
	</button>
{/if}
