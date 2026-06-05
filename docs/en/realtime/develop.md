---
title: Development Guide
sidebar_position: 3
---

## Client Development Guide

#### Prerequisites

1. **Install gRPC and Protocol Buffers**

   Ensure gRPC and the Protocol Buffers (`protoc`) compiler are installed correctly in your development environment. Follow the official documentation for installation and version compatibility.

2. **Generate client code**

   Use the MipMap `.proto` file [Proto file download](https://asset.mipmap3d.com/engine/realtime.proto) and generate client code for your target language with `protoc`. See the gRPC official documentation or in-project instructions for commands and options.

3. **Prepare the task configuration JSON file**

   The real-time reconstruction service requires a task configuration JSON file at initialization. Ensure the file is complete, the path is correct, and the server can access it.

:::tip Path access
Ensure image paths in the client configuration are local or network paths directly accessible by the server. Inaccessible paths will cause task failures.
:::

#### Standard real-time reconstruction workflow

1. **Start the server (`realtime_service`)**

Start the server and keep it listening. Specify the port with `-port_number`, for example:

```bash
realtime_service -port_number 50051
```

Multiple server instances can run concurrently on different ports.

2. **Start the client and connect**

Start the client, specify the server port, and establish a gRPC connection. Call interfaces in this order:

- `Initialize` — Initialize the real-time reconstruction task
- `AddFrame` — Add image frames one at a time
- `EndTask` — End the current real-time task

3. **Task management and repeated runs**

The server process can run continuously and handle multiple tasks. The client exits after each `EndTask` call. To start a new task, launch the client again, connect to the server, and repeat the workflow above.

#### Client program `realtime_client`

The `realtime_client` program is provided for callers who are unfamiliar with gRPC and have a simple integration pattern. No custom client code is required—start `realtime_client` and the server executable.

Steps:

(1) Add `image_dir` and the target scaled image size to the initialization JSON. Ensure the directory is valid.

| Field | Type | Description |
|----------------------|--------------|--------------------------------------------------------------|
| `image_dir` | string | Image folder path (UTF-8 encoding) |
| `target_max_size` | int | Longest-edge pixel count after frame scaling. Real-time tasks should scale frames; larger values improve quality but increase per-frame latency. When `0`, scale images externally and pass the scaled image metadata string (see below). |

(2) Start `realtime_service` from the console. The optional argument is the port number; different ports represent different services. Default port: `50051`.

```bash
realtime_service -port_number 50051
```

(3) Start `realtime_client` from the console with the same optional port argument.

```bash
realtime_client -port_number 50051
```

After the client starts, the console prompts for the path to the parameter JSON file. Enter the JSON file path (without quotes). The server then runs real-time reconstruction and prints logs.

(4) Poll 2D and 3D real-time outputs on a schedule and refresh rendering.

(5) After the real-time task starts, the client waits for the `end` command. When you type `end`, the server stops adding frames and ends the current real-time task.

### Miaosuan edition development guide

The Miaosuan edition SDK has special requirements:

1. Set the `realtime_service` `task_type` parameter to `2D`:

```bash
./realtime_service --port_number 50051(optional) --task_type 2D
```

2. The `license_id` field must be `9202`.

3. `AddFrame` `image_meta_data` must be a valid value—the caller must scale images and supply metadata for the scaled images. Use the `extract_image_info` utility:

```bash
./extract_image_info --src_image_path path_to_src_image --dst_image_path path_to_dst_image --info_json path_to_extracted_image_meta_data --max_side_width target_pixels_of_longest_side_of_dst_iamge
```

4. The following JSON parameters have no effect during server initialization: `do_realtime_3d_map`, `target_max_size`, `mesh_3d_save_type`, `rt_mesh_reso_scale_to_gsd`, `save_dom_tiff`, `coordinate_descriptor_2D`.
