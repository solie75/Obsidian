`TSoftObjectPtr`의 `IsPending()` 함수는 소프트 레퍼런스가 가리키는 객체가 아직 로드되지 않았는지를 확인하는 데 사용됩니다.

`IsPending()` 함수는 소프트 레퍼런스가 가리키는 객체가 아직 로드되지 않은 상태인지 확인하는 함수입니다. 이 함수는 객체가 아직 로드되지 않아서, 로드가 대기 중인 경우에 `true`를 반환합니다.

```c++
UPROPERTY()
TSoftObjectPtr<UStaticMesh> MeshPtr;

void CheckMeshStatus()
{
    if (MeshPtr.IsPending())
    {
        UE_LOG(LogTemp, Warning, TEXT("Mesh is pending load."));
    }
    else if (MeshPtr.IsValid())
    {
        UStaticMesh* Mesh = MeshPtr.Get();
        UE_LOG(LogTemp, Warning, TEXT("Mesh is loaded and valid."));
    }
    else
    {
        UE_LOG(LogTemp, Warning, TEXT("Mesh is not valid and not pending."));
    }
}

```

- **IsPending()**: 객체가 아직 로드되지 않은 상태인지 확인합니다. 객체가 로드 대기 중인 경우 `true`를 반환합니다.
- **IsValid()**: 소프트 레퍼런스가 유효한지 확인합니다. 객체가 이미 로드되어 메모리에 있는 경우 `true`를 반환합니다.
- **Get()**: 소프트 레퍼런스가 가리키는 실제 객체를 반환합니다. 객체가 로드되지 않았으면 `nullptr`를 반환합니다.

# 주의

- **비동기 로드**: 소프트 레퍼런스는 비동기적으로 객체를 로드할 수 있습니다. `IsPending()`을 사용하여 객체가 아직 로드되지 않았는지 확인하고, 필요할 때 비동기 로드를 트리거할 수 있습니다.
- **메모리 관리**: 소프트 레퍼런스를 사용하면 필요할 때만 객체를 로드하므로 메모리 사용량을 줄일 수 있습니다. 하지만 비동기 로드가 완료되기 전에 객체에 접근하려고 하면 문제가 발생할 수 있습니다.
# 결론

- **`IsPending()`**: 소프트 레퍼런스가 가리키는 객체가 아직 로드되지 않았는지 확인합니다.
- **사용 상황**: 객체의 로드 상태를 확인하고, 필요할 때 로드를 트리거하기 위해 사용합니다.
- **메모리 및 성능 관리**: 소프트 레퍼런스를 통해 메모리 사용량을 줄이고, 필요한 시점에만 객체를 로드하여 성능을 최적화할 수 있습니다.