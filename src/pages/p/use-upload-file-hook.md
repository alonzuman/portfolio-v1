---
title: useUploadFile hook for Firebase
date: July 24th, 2022
layout: ../../layouts/Layout.astro
---

A simple hook for uploading a new file to store in firebase storage

```typescript
const useUploadFile = () => {
	const [progress, setProgress] = useState<number | null>(null);
	const [downloadUrl, setDownloadUrl] = useState("");

	return {
		downloadUrl,
		setDownloadUrl,
		progress,
		isLoading: typeof progress === "number" && progress > 0 && progress < 100,
		upload: async (file: Blob | File, path: string) => {
			const storageRef = ref(storage, path);
			const taskRef = uploadBytesResumable(storageRef, file);

			taskRef.on(
				"state_changed",
				(snapshot) => {
					// Observe state change events such as progress, pause, and resume
					// Get task progress, including the number of bytes uploaded and the total number of bytes to be uploaded
					const _progress =
						(snapshot.bytesTransferred / snapshot.totalBytes) * 100;
					setProgress(_progress);
				},
				(error) => {
					// Handle unsuccessful uploads
					alert(JSON.stringify({ error }));
					setProgress(null);
				},
				() => {
					// Handle successful uploads on complete
					// For instance, get the download URL: https://firebasestorage.googleapis.com/...
					getDownloadURL(taskRef.snapshot.ref).then((_downloadUrl) => {
						setDownloadUrl(_downloadUrl);
					});
					setProgress(null);
				}
			);
		},
	};
};
```
